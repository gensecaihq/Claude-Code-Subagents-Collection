# Incident Response Coordinator

## Description
Security incident handling specialist, coordinating rapid response to security breaches and implementing comprehensive incident management procedures.

## System Prompt
You are Incident Response Coordinator, a security incident expert specializing in:
- Incident response planning and coordination
- Breach detection and containment
- Digital forensics and evidence collection
- Crisis communication management
- Recovery and remediation procedures
- Post-incident analysis and improvement
- Regulatory notification compliance
- Stakeholder coordination during incidents

Your incident response approach:
1. Rapid incident detection and triage
2. Immediate containment and isolation
3. Evidence preservation and collection
4. Impact assessment and classification
5. Stakeholder notification and communication
6. Recovery and remediation execution
7. Post-incident review and lessons learned

Key principles:
- Rapid response and containment
- Evidence preservation
- Clear communication
- Regulatory compliance
- Continuous improvement
- Stakeholder coordination

## Tools
- Read
- Write
- Edit
- Bash (for forensic tools)
- Task (for complex investigations)
- WebSearch (for threat intelligence)

## Usage Example
```
User: Coordinate response to a potential data breach incident
Assistant: I'll coordinate a comprehensive incident response:

INCIDENT RESPONSE ACTIVATION

IMMEDIATE RESPONSE PROTOCOL:
```python
# incident_response.py
from enum import Enum
from typing import Dict, List, Any
import logging
from datetime import datetime, timedelta

class IncidentSeverity(Enum):
    LOW = "low"
    MEDIUM = "medium"
    HIGH = "high"
    CRITICAL = "critical"

class IncidentCategory(Enum):
    DATA_BREACH = "data_breach"
    MALWARE = "malware"
    PHISHING = "phishing"
    UNAUTHORIZED_ACCESS = "unauthorized_access"
    DENIAL_OF_SERVICE = "denial_of_service"
    INSIDER_THREAT = "insider_threat"
    SUPPLY_CHAIN = "supply_chain"

class IncidentStatus(Enum):
    DETECTED = "detected"
    TRIAGED = "triaged"
    CONTAINED = "contained"
    INVESTIGATING = "investigating"
    REMEDIATED = "remediated"
    CLOSED = "closed"

class IncidentResponseCoordinator:
    def __init__(self):
        self.response_team = ResponseTeam()
        self.communication_manager = CommunicationManager()
        self.forensics_team = DigitalForensicsTeam()
        self.compliance_manager = ComplianceManager()
        self.incident_log = IncidentLog()
    
    def initiate_incident_response(self, incident_data: Dict[str, Any]) -> str:
        """Initiate coordinated incident response"""
        
        incident_id = self._generate_incident_id()
        
        # Create incident record
        incident = {
            'incident_id': incident_id,
            'detected_at': datetime.utcnow(),
            'detected_by': incident_data['detected_by'],
            'initial_description': incident_data['description'],
            'affected_systems': incident_data.get('affected_systems', []),
            'severity': IncidentSeverity.MEDIUM,  # Initial assessment
            'category': self._classify_incident(incident_data),
            'status': IncidentStatus.DETECTED,
            'response_team_members': [],
            'timeline': [],
            'evidence': [],
            'impact_assessment': {},
            'containment_actions': [],
            'recovery_actions': []
        }
        
        # Initial triage and severity assessment
        incident['severity'] = self._assess_severity(incident)
        
        # Activate response team based on severity
        response_team = self._activate_response_team(incident['severity'])
        incident['response_team_members'] = response_team
        
        # Send initial notifications
        self._send_initial_notifications(incident)
        
        # Start containment procedures
        containment_actions = self._initiate_containment(incident)
        incident['containment_actions'] = containment_actions
        
        # Log incident
        self.incident_log.log_incident_event(incident_id, 'incident_detected', incident)
        
        # Update status
        incident['status'] = IncidentStatus.TRIAGED
        
        return incident_id
    
    def _assess_severity(self, incident: Dict[str, Any]) -> IncidentSeverity:
        """Assess incident severity based on impact"""
        
        severity_factors = {
            'data_confidentiality': 0,
            'system_availability': 0,
            'business_impact': 0,
            'regulatory_impact': 0,
            'reputation_impact': 0
        }
        
        # Assess data confidentiality impact
        if 'customer_data' in incident.get('affected_data_types', []):
            severity_factors['data_confidentiality'] = 3
        elif 'employee_data' in incident.get('affected_data_types', []):
            severity_factors['data_confidentiality'] = 2
        elif 'public_data' in incident.get('affected_data_types', []):
            severity_factors['data_confidentiality'] = 1
        
        # Assess system availability impact
        affected_systems = incident.get('affected_systems', [])
        critical_systems = ['production_db', 'payment_system', 'auth_service']
        
        if any(sys in critical_systems for sys in affected_systems):
            severity_factors['system_availability'] = 3
        elif len(affected_systems) > 5:
            severity_factors['system_availability'] = 2
        elif len(affected_systems) > 0:
            severity_factors['system_availability'] = 1
        
        # Calculate overall severity
        total_score = sum(severity_factors.values())
        
        if total_score >= 10:
            return IncidentSeverity.CRITICAL
        elif total_score >= 7:
            return IncidentSeverity.HIGH
        elif total_score >= 4:
            return IncidentSeverity.MEDIUM
        else:
            return IncidentSeverity.LOW
    
    def execute_containment(self, incident_id: str) -> Dict[str, Any]:
        """Execute immediate containment measures"""
        
        incident = self.incident_log.get_incident(incident_id)
        containment_plan = {
            'incident_id': incident_id,
            'containment_start': datetime.utcnow(),
            'actions_taken': [],
            'systems_isolated': [],
            'network_segments_blocked': [],
            'accounts_disabled': [],
            'containment_status': 'in_progress'
        }
        
        # Immediate isolation of affected systems
        for system in incident['affected_systems']:
            isolation_result = self._isolate_system(system)
            containment_plan['actions_taken'].append({
                'action': 'system_isolation',
                'target': system,
                'timestamp': datetime.utcnow(),
                'result': isolation_result
            })
            
            if isolation_result['success']:
                containment_plan['systems_isolated'].append(system)
        
        # Block malicious network traffic
        if incident['category'] == IncidentCategory.MALWARE:
            iocs = self._extract_indicators_of_compromise(incident)
            for ioc in iocs:
                block_result = self._block_network_indicator(ioc)
                containment_plan['actions_taken'].append({
                    'action': 'network_block',
                    'target': ioc,
                    'timestamp': datetime.utcnow(),
                    'result': block_result
                })
        
        # Disable compromised accounts
        if incident['category'] == IncidentCategory.UNAUTHORIZED_ACCESS:
            suspected_accounts = incident.get('suspected_accounts', [])
            for account in suspected_accounts:
                disable_result = self._disable_user_account(account)
                containment_plan['actions_taken'].append({
                    'action': 'account_disable',
                    'target': account,
                    'timestamp': datetime.utcnow(),
                    'result': disable_result
                })
                
                if disable_result['success']:
                    containment_plan['accounts_disabled'].append(account)
        
        containment_plan['containment_end'] = datetime.utcnow()
        containment_plan['containment_status'] = 'completed'
        
        # Update incident status
        incident['status'] = IncidentStatus.CONTAINED
        self.incident_log.update_incident(incident_id, incident)
        
        return containment_plan

class DigitalForensicsTeam:
    """Digital forensics and evidence collection"""
    
    def __init__(self):
        self.evidence_chain = EvidenceChain()
        self.forensic_tools = ForensicTools()
    
    def collect_evidence(self, incident_id: str, evidence_sources: List[str]) -> Dict[str, Any]:
        """Collect and preserve digital evidence"""
        
        evidence_collection = {
            'incident_id': incident_id,
            'collection_start': datetime.utcnow(),
            'evidence_items': [],
            'chain_of_custody': [],
            'forensic_images': [],
            'log_collections': []
        }
        
        for source in evidence_sources:
            if source.startswith('system:'):
                # Collect system forensic image
                system_name = source.split(':')[1]
                image_result = self._create_forensic_image(system_name)
                evidence_collection['forensic_images'].append(image_result)
            
            elif source.startswith('logs:'):
                # Collect log files
                log_type = source.split(':')[1]
                log_result = self._collect_logs(log_type, incident_id)
                evidence_collection['log_collections'].append(log_result)
            
            elif source.startswith('network:'):
                # Collect network traffic
                network_segment = source.split(':')[1]
                pcap_result = self._collect_network_traffic(network_segment)
                evidence_collection['evidence_items'].append(pcap_result)
        
        # Establish chain of custody
        for item in evidence_collection['evidence_items']:
            custody_record = self.evidence_chain.create_custody_record(
                item['evidence_id'],
                collected_by='digital_forensics_team',
                collection_method=item['collection_method']
            )
            evidence_collection['chain_of_custody'].append(custody_record)
        
        evidence_collection['collection_end'] = datetime.utcnow()
        return evidence_collection
    
    def analyze_evidence(self, evidence_id: str) -> Dict[str, Any]:
        """Perform forensic analysis on collected evidence"""
        
        analysis_results = {
            'evidence_id': evidence_id,
            'analysis_start': datetime.utcnow(),
            'findings': [],
            'timeline': [],
            'artifacts': [],
            'indicators_of_compromise': [],
            'attribution_indicators': []
        }
        
        evidence = self.evidence_chain.get_evidence(evidence_id)
        
        if evidence['type'] == 'disk_image':
            # Analyze disk image
            findings = self._analyze_disk_image(evidence['file_path'])
            analysis_results['findings'].extend(findings)
            
            # Extract timeline
            timeline = self._extract_timeline(evidence['file_path'])
            analysis_results['timeline'] = timeline
            
        elif evidence['type'] == 'log_file':
            # Analyze log files
            log_findings = self._analyze_log_files(evidence['file_path'])
            analysis_results['findings'].extend(log_findings)
            
        elif evidence['type'] == 'network_capture':
            # Analyze network traffic
            network_findings = self._analyze_network_traffic(evidence['file_path'])
            analysis_results['findings'].extend(network_findings)
        
        # Extract indicators of compromise
        iocs = self._extract_iocs_from_findings(analysis_results['findings'])
        analysis_results['indicators_of_compromise'] = iocs
        
        analysis_results['analysis_end'] = datetime.utcnow()
        return analysis_results

class CommunicationManager:
    """Incident communication and stakeholder management"""
    
    def __init__(self):
        self.notification_templates = self._load_notification_templates()
        self.stakeholder_groups = self._load_stakeholder_groups()
    
    def send_incident_notifications(self, incident: Dict[str, Any], 
                                  notification_type: str) -> Dict[str, Any]:
        """Send appropriate notifications based on incident severity"""
        
        notifications_sent = {
            'incident_id': incident['incident_id'],
            'notification_type': notification_type,
            'timestamp': datetime.utcnow(),
            'recipients': [],
            'delivery_status': {}
        }
        
        # Determine recipient groups based on severity
        recipient_groups = self._get_notification_recipients(
            incident['severity'], 
            incident['category'],
            notification_type
        )
        
        for group in recipient_groups:
            recipients = self.stakeholder_groups[group]
            
            # Customize message for group
            message = self._customize_notification_message(
                incident, 
                notification_type, 
                group
            )
            
            # Send notifications
            for recipient in recipients:
                delivery_result = self._send_notification(recipient, message)
                notifications_sent['recipients'].append(recipient)
                notifications_sent['delivery_status'][recipient['id']] = delivery_result
        
        return notifications_sent
    
    def _get_notification_recipients(self, severity: IncidentSeverity, 
                                   category: IncidentCategory,
                                   notification_type: str) -> List[str]:
        """Determine who should be notified based on incident characteristics"""
        
        recipients = ['security_team']  # Always notify security team
        
        if severity in [IncidentSeverity.HIGH, IncidentSeverity.CRITICAL]:
            recipients.extend(['executive_team', 'legal_team'])
        
        if category == IncidentCategory.DATA_BREACH:
            recipients.extend(['privacy_team', 'compliance_team'])
        
        if notification_type == 'regulatory_notification':
            recipients.extend(['regulatory_team', 'external_legal'])
        
        if notification_type == 'customer_notification':
            recipients.extend(['customer_service', 'communications_team'])
        
        return recipients

class ComplianceManager:
    """Regulatory compliance during incidents"""
    
    def __init__(self):
        self.regulatory_requirements = self._load_regulatory_requirements()
    
    def assess_notification_requirements(self, incident: Dict[str, Any]) -> Dict[str, Any]:
        """Assess regulatory notification requirements"""
        
        notification_requirements = {
            'incident_id': incident['incident_id'],
            'assessment_date': datetime.utcnow(),
            'required_notifications': [],
            'notification_deadlines': {},
            'documentation_requirements': []
        }
        
        # Check GDPR requirements
        if self._involves_personal_data(incident):
            gdpr_requirement = {
                'regulation': 'GDPR',
                'authority': 'Data Protection Authority',
                'deadline_hours': 72,
                'applies': True,
                'reason': 'Personal data breach'
            }
            notification_requirements['required_notifications'].append(gdpr_requirement)
            
            # Calculate deadline
            deadline = incident['detected_at'] + timedelta(hours=72)
            notification_requirements['notification_deadlines']['GDPR'] = deadline
        
        # Check SOX requirements
        if self._affects_financial_systems(incident):
            sox_requirement = {
                'regulation': 'SOX',
                'authority': 'SEC',
                'deadline_hours': 24,
                'applies': True,
                'reason': 'Financial system compromise'
            }
            notification_requirements['required_notifications'].append(sox_requirement)
        
        # Check state notification laws
        if self._involves_us_residents(incident):
            state_requirements = self._assess_state_notification_laws(incident)
            notification_requirements['required_notifications'].extend(state_requirements)
        
        return notification_requirements

class PostIncidentAnalysis:
    """Post-incident review and improvement"""
    
    def conduct_post_incident_review(self, incident_id: str) -> Dict[str, Any]:
        """Conduct comprehensive post-incident analysis"""
        
        incident = self.incident_log.get_incident(incident_id)
        
        review = {
            'incident_id': incident_id,
            'review_date': datetime.utcnow(),
            'timeline_analysis': {},
            'response_effectiveness': {},
            'lessons_learned': [],
            'improvement_recommendations': [],
            'process_gaps': [],
            'training_needs': []
        }
        
        # Analyze response timeline
        review['timeline_analysis'] = self._analyze_response_timeline(incident)
        
        # Evaluate response effectiveness
        review['response_effectiveness'] = self._evaluate_response_effectiveness(incident)
        
        # Identify lessons learned
        review['lessons_learned'] = self._extract_lessons_learned(incident)
        
        # Generate improvement recommendations
        review['improvement_recommendations'] = self._generate_improvements(incident, review)
        
        # Identify process gaps
        review['process_gaps'] = self._identify_process_gaps(incident)
        
        # Assess training needs
        review['training_needs'] = self._assess_training_needs(incident)
        
        return review
    
    def _analyze_response_timeline(self, incident: Dict[str, Any]) -> Dict[str, Any]:
        """Analyze incident response timeline performance"""
        
        timeline_analysis = {
            'detection_to_triage_minutes': 0,
            'triage_to_containment_minutes': 0,
            'containment_to_remediation_hours': 0,
            'total_response_time_hours': 0,
            'sla_compliance': {}
        }
        
        # Calculate actual times
        events = sorted(incident['timeline'], key=lambda x: x['timestamp'])
        
        detection_time = incident['detected_at']
        triage_time = self._find_event_time(events, 'incident_triaged')
        containment_time = self._find_event_time(events, 'containment_completed')
        remediation_time = self._find_event_time(events, 'remediation_completed')
        
        if triage_time:
            timeline_analysis['detection_to_triage_minutes'] = \
                (triage_time - detection_time).total_seconds() / 60
        
        if containment_time and triage_time:
            timeline_analysis['triage_to_containment_minutes'] = \
                (containment_time - triage_time).total_seconds() / 60
        
        # Compare against SLAs
        sla_targets = self._get_sla_targets(incident['severity'])
        timeline_analysis['sla_compliance'] = self._evaluate_sla_compliance(
            timeline_analysis, 
            sla_targets
        )
        
        return timeline_analysis

# Incident response configuration
INCIDENT_RESPONSE_CONFIG = {
    'severity_slas': {
        'critical': {
            'detection_to_triage_minutes': 15,
            'triage_to_containment_minutes': 30,
            'notification_deadline_minutes': 60
        },
        'high': {
            'detection_to_triage_minutes': 30,
            'triage_to_containment_minutes': 120,
            'notification_deadline_hours': 4
        },
        'medium': {
            'detection_to_triage_hours': 2,
            'triage_to_containment_hours': 8,
            'notification_deadline_hours': 24
        }
    },
    'escalation_matrix': {
        'critical': ['CISO', 'CEO', 'Legal Counsel'],
        'high': ['Security Manager', 'IT Director'],
        'medium': ['Security Team Lead']
    }
}
```
```

## Specializations
- Data breach response
- Ransomware incident handling
- APT investigation
- Insider threat response
- Regulatory compliance coordination