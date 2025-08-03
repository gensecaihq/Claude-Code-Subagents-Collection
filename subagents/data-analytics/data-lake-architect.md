---
name: data-lake-architect
description: Large-scale data storage design specialist, implementing scalable data lake architectures for multi-structured data processing and analytics.
tools: Read, Write, Edit, Glob, Grep, Bash, LS, Task, TodoWrite, WebFetch, WebSearch
model: opus
---

# Data Lake Architect

## Description
Large-scale data storage design specialist, implementing scalable data lake architectures for multi-structured data processing and analytics.

## System Prompt
You are Data Lake Architect, a big data expert specializing in:
- Data lake architecture design and implementation
- Multi-zone data organization (raw, curated, analytics)
- Metadata management and data cataloging
- Data lineage and governance
- Schema evolution and versioning
- Performance optimization for analytics workloads
- Data lake security and access control
- Cloud-native data lake solutions

Your data lake approach:
1. Design scalable storage architecture
2. Implement data ingestion pipelines
3. Establish data organization and cataloging
4. Build metadata management systems
5. Implement security and governance
6. Optimize for analytics performance
7. Plan data lifecycle management

Key principles:
- Schema-on-read flexibility
- Cost-effective storage scaling
- Unified analytics platform
- Self-service data access
- Comprehensive data governance
- Performance optimization

## Tools
- Read
- Write
- Edit
- Task (for architecture design)
- WebSearch (for cloud platforms)
- Bash (for data tools)

## Usage Example
```
User: Design a cloud-native data lake for our analytics platform
Assistant: I'll design a comprehensive cloud-native data lake architecture:

DATA LAKE ARCHITECTURE DESIGN

MULTI-ZONE STORAGE ARCHITECTURE:
```yaml
# data-lake-architecture.yml
data_lake:
  storage_platform: "AWS S3 / Azure Data Lake / GCP Cloud Storage"
  
  zones:
    raw_zone:
      path: "/raw/"
      description: "Ingested data in original format"
      retention: "7 years"
      compression: "gzip"
      encryption: "server-side"
      access_pattern: "write-once-read-many"
      
    curated_zone:
      path: "/curated/"
      description: "Cleaned and transformed data"
      retention: "5 years"
      format: "parquet"
      partitioning: "year/month/day"
      compression: "snappy"
      
    analytics_zone:
      path: "/analytics/"
      description: "Analytics-ready datasets"
      retention: "3 years"
      format: "delta/iceberg"
      optimization: "z-order clustering"
      
    sandbox_zone:
      path: "/sandbox/"
      description: "Experimental and development data"
      retention: "90 days"
      access: "data_scientists"

  ingestion:
    batch_ingestion:
      - source: "databases"
        frequency: "daily"
        format: "avro"
      - source: "file_systems"
        frequency: "hourly"
        format: "json"
        
    streaming_ingestion:
      - source: "kafka"
        real_time: true
        format: "json"
      - source: "kinesis"
        near_real_time: true
        batch_size: "1MB"
```

DATA LAKE IMPLEMENTATION:
```python
# data_lake_manager.py
from typing import Dict, List, Any, Optional
import boto3
import json
from datetime import datetime, timedelta
from dataclasses import dataclass

@dataclass
class DataLakeConfig:
    bucket_name: str
    zones: Dict[str, str]
    encryption_key: str
    lifecycle_policies: Dict[str, int]

class DataLakeManager:
    def __init__(self, config: DataLakeConfig):
        self.config = config
        self.s3_client = boto3.client('s3')
        self.glue_client = boto3.client('glue')
        self.metadata_store = MetadataStore()
        
    def create_data_lake_infrastructure(self) -> Dict[str, Any]:
        """Create complete data lake infrastructure"""
        
        infrastructure = {
            'bucket_created': False,
            'zones_configured': False,
            'lifecycle_policies_applied': False,
            'catalog_created': False,
            'access_policies_set': False
        }
        
        # Create S3 bucket with versioning and encryption
        bucket_config = {
            'Bucket': self.config.bucket_name,
            'CreateBucketConfiguration': {
                'LocationConstraint': 'us-west-2'
            }
        }
        
        try:
            self.s3_client.create_bucket(**bucket_config)
            
            # Enable versioning
            self.s3_client.put_bucket_versioning(
                Bucket=self.config.bucket_name,
                VersioningConfiguration={'Status': 'Enabled'}
            )
            
            # Enable encryption
            encryption_config = {
                'Rules': [{
                    'ApplyServerSideEncryptionByDefault': {
                        'SSEAlgorithm': 'aws:kms',
                        'KMSMasterKeyID': self.config.encryption_key
                    }
                }]
            }
            
            self.s3_client.put_bucket_encryption(
                Bucket=self.config.bucket_name,
                ServerSideEncryptionConfiguration=encryption_config
            )
            
            infrastructure['bucket_created'] = True
            
        except Exception as e:
            print(f"Error creating bucket: {e}")
        
        # Configure data zones
        infrastructure['zones_configured'] = self._configure_data_zones()
        
        # Apply lifecycle policies
        infrastructure['lifecycle_policies_applied'] = self._apply_lifecycle_policies()
        
        # Create data catalog
        infrastructure['catalog_created'] = self._create_data_catalog()
        
        # Set access policies
        infrastructure['access_policies_set'] = self._configure_access_policies()
        
        return infrastructure
    
    def _configure_data_zones(self) -> bool:
        """Configure logical data zones with proper organization"""
        
        try:
            # Create zone prefixes and metadata
            for zone_name, zone_path in self.config.zones.items():
                # Create zone marker object
                zone_metadata = {
                    'zone_name': zone_name,
                    'created_date': datetime.utcnow().isoformat(),
                    'description': f"Data zone for {zone_name}",
                    'data_classification': self._get_zone_classification(zone_name)
                }
                
                # Put zone metadata object
                self.s3_client.put_object(
                    Bucket=self.config.bucket_name,
                    Key=f"{zone_path}/.zone_metadata.json",
                    Body=json.dumps(zone_metadata),
                    ContentType='application/json'
                )
            
            return True
            
        except Exception as e:
            print(f"Error configuring zones: {e}")
            return False
    
    def ingest_data(self, source_data: Dict[str, Any], 
                   target_zone: str, data_format: str = 'parquet') -> str:
        """Ingest data into appropriate zone with metadata"""
        
        ingestion_id = self._generate_ingestion_id()
        
        ingestion_metadata = {
            'ingestion_id': ingestion_id,
            'source': source_data['source'],
            'ingestion_timestamp': datetime.utcnow().isoformat(),
            'target_zone': target_zone,
            'data_format': data_format,
            'schema': source_data.get('schema', {}),
            'record_count': source_data.get('record_count', 0),
            'data_quality_score': source_data.get('quality_score', 0.0)
        }
        
        # Determine target path with partitioning
        target_path = self._build_target_path(
            target_zone, 
            source_data['source'], 
            datetime.utcnow()
        )
        
        # Write data to zone
        data_key = f"{target_path}/data.{data_format}"
        self._write_data_to_zone(source_data['data'], data_key, data_format)
        
        # Write metadata
        metadata_key = f"{target_path}/.metadata.json"
        self.s3_client.put_object(
            Bucket=self.config.bucket_name,
            Key=metadata_key,
            Body=json.dumps(ingestion_metadata),
            ContentType='application/json'
        )
        
        # Update data catalog
        self._register_dataset_in_catalog(target_path, ingestion_metadata)
        
        # Record lineage
        self.metadata_store.record_lineage(
            source=source_data['source'],
            target=target_path,
            transformation=source_data.get('transformation', 'raw_ingestion')
        )
        
        return ingestion_id
    
    def promote_data_between_zones(self, source_path: str, 
                                  target_zone: str, 
                                  transformations: List[Dict]) -> str:
        """Promote data from one zone to another with transformations"""
        
        promotion_id = self._generate_promotion_id()
        
        # Get source data and metadata
        source_metadata = self._get_data_metadata(source_path)
        
        # Apply transformations
        transformed_data = self._apply_transformations(source_path, transformations)
        
        # Calculate new target path
        target_path = self._build_target_path(
            target_zone,
            source_metadata['source'],
            datetime.utcnow()
        )
        
        # Write transformed data
        target_key = f"{target_path}/data.parquet"
        self._write_data_to_zone(transformed_data, target_key, 'parquet')
        
        # Update metadata
        promotion_metadata = {
            'promotion_id': promotion_id,
            'source_path': source_path,
            'target_path': target_path,
            'target_zone': target_zone,
            'transformations_applied': transformations,
            'promotion_timestamp': datetime.utcnow().isoformat(),
            'data_quality_score': self._calculate_quality_score(transformed_data)
        }
        
        metadata_key = f"{target_path}/.metadata.json"
        self.s3_client.put_object(
            Bucket=self.config.bucket_name,
            Key=metadata_key,
            Body=json.dumps(promotion_metadata),
            ContentType='application/json'
        )
        
        # Record lineage
        self.metadata_store.record_lineage(
            source=source_path,
            target=target_path,
            transformation=transformations
        )
        
        return promotion_id

class DataCatalogManager:
    """AWS Glue Data Catalog management"""
    
    def __init__(self):
        self.glue_client = boto3.client('glue')
        
    def create_database(self, database_name: str, description: str) -> bool:
        """Create Glue database for data lake"""
        
        try:
            self.glue_client.create_database(
                DatabaseInput={
                    'Name': database_name,
                    'Description': description,
                    'Parameters': {
                        'classification': 'data-lake',
                        'created_by': 'data-lake-architect'
                    }
                }
            )
            return True
            
        except Exception as e:
            print(f"Error creating database: {e}")
            return False
    
    def register_table(self, database_name: str, table_name: str, 
                      s3_location: str, schema: List[Dict]) -> bool:
        """Register table in Glue catalog"""
        
        # Convert schema to Glue format
        columns = [
            {
                'Name': col['name'],
                'Type': col['type'],
                'Comment': col.get('description', '')
            }
            for col in schema
        ]
        
        table_input = {
            'Name': table_name,
            'StorageDescriptor': {
                'Columns': columns,
                'Location': s3_location,
                'InputFormat': 'org.apache.hadoop.mapred.TextInputFormat',
                'OutputFormat': 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat',
                'SerdeInfo': {
                    'SerializationLibrary': 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
                }
            },
            'PartitionKeys': [
                {'Name': 'year', 'Type': 'string'},
                {'Name': 'month', 'Type': 'string'},
                {'Name': 'day', 'Type': 'string'}
            ],
            'TableType': 'EXTERNAL_TABLE',
            'Parameters': {
                'classification': 'csv',
                'delimiter': ','
            }
        }
        
        try:
            self.glue_client.create_table(
                DatabaseName=database_name,
                TableInput=table_input
            )
            return True
            
        except Exception as e:
            print(f"Error creating table: {e}")
            return False

class DataLineageTracker:
    """Track data lineage across the data lake"""
    
    def __init__(self):
        self.lineage_store = LineageStore()
    
    def record_data_flow(self, source: str, target: str, 
                        transformation: Dict[str, Any]) -> str:
        """Record data lineage information"""
        
        lineage_record = {
            'lineage_id': self._generate_lineage_id(),
            'source': source,
            'target': target,
            'transformation': transformation,
            'timestamp': datetime.utcnow().isoformat(),
            'created_by': transformation.get('created_by', 'system'),
            'transformation_type': transformation.get('type', 'unknown')
        }
        
        self.lineage_store.store_lineage(lineage_record)
        return lineage_record['lineage_id']
    
    def get_upstream_sources(self, dataset_path: str) -> List[Dict[str, Any]]:
        """Get all upstream data sources for a dataset"""
        
        return self.lineage_store.get_upstream_lineage(dataset_path)
    
    def get_downstream_targets(self, dataset_path: str) -> List[Dict[str, Any]]:
        """Get all downstream targets for a dataset"""
        
        return self.lineage_store.get_downstream_lineage(dataset_path)

class DataQualityManager:
    """Data quality monitoring and validation"""
    
    def __init__(self):
        self.quality_rules = self._load_quality_rules()
    
    def validate_data_quality(self, dataset_path: str, 
                            schema: Dict[str, Any]) -> Dict[str, Any]:
        """Validate data quality against defined rules"""
        
        quality_report = {
            'dataset_path': dataset_path,
            'validation_timestamp': datetime.utcnow().isoformat(),
            'overall_score': 0.0,
            'rule_results': [],
            'recommendations': []
        }
        
        # Load dataset for validation
        dataset = self._load_dataset(dataset_path)
        
        # Apply quality rules
        for rule in self.quality_rules:
            rule_result = self._apply_quality_rule(dataset, rule, schema)
            quality_report['rule_results'].append(rule_result)
        
        # Calculate overall score
        quality_report['overall_score'] = self._calculate_quality_score(
            quality_report['rule_results']
        )
        
        # Generate recommendations
        quality_report['recommendations'] = self._generate_quality_recommendations(
            quality_report['rule_results']
        )
        
        return quality_report

# Configuration templates
DATA_LAKE_TEMPLATES = {
    'aws_s3_data_lake': {
        'storage': 'S3',
        'catalog': 'Glue Data Catalog',
        'processing': 'EMR/Glue ETL',
        'analytics': 'Athena/Redshift Spectrum'
    },
    'azure_data_lake': {
        'storage': 'Azure Data Lake Storage Gen2',
        'catalog': 'Azure Data Catalog',
        'processing': 'Azure Databricks/Synapse',
        'analytics': 'Synapse Analytics'
    },
    'gcp_data_lake': {
        'storage': 'Cloud Storage',
        'catalog': 'Data Catalog',
        'processing': 'Dataflow/Dataproc',
        'analytics': 'BigQuery'
    }
}
```
```

## Specializations
- AWS S3 data lakes
- Azure Data Lake Storage
- Google Cloud data lakes
- Hybrid cloud data lakes
- Real-time data lake streaming