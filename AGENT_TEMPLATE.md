# Agent Template

This template provides the standardized format for creating professional-grade sub-agents based on industry best practices and proven patterns.

## Standard Agent File Format

```yaml
---
name: agent-name
description: Clear description of when this agent should be invoked and what specific problems it solves
tools: Read, Write, Edit, Glob, Grep, Bash, LS, Task, TodoWrite, WebFetch, WebSearch
model: sonnet  # Use 'opus' for complex reasoning tasks, 'sonnet' for standard development
---

# Agent Name

## Role & Expertise
Clear definition of the agent's primary role and domain expertise. What specific problems does this agent solve?

## Key Capabilities
- Bulleted list of specific abilities
- Concrete technical skills
- Measurable outcomes the agent can deliver
- Integration patterns with other tools/agents

## Core Competencies

### Technical Knowledge Areas
**Framework/Technology Mastery:**
- Specific versions and features
- Advanced patterns and best practices
- Performance optimization techniques
- Security considerations

**Industry Standards:**
- Compliance frameworks (if applicable)
- Quality metrics and benchmarks
- Professional methodologies

### Specialized Skills
**Domain-Specific Expertise:**
- Advanced techniques and patterns
- Tool integration and automation
- Problem-solving methodologies

## Standard Operating Procedure

### Phase 1: Context Acquisition
1. **Project Analysis**: Query @project-analyzer for technology stack context
2. **Requirement Gathering**: Understand specific task requirements and constraints
3. **Environment Assessment**: Analyze existing codebase and patterns

### Phase 2: Execution Planning
1. **Approach Selection**: Choose optimal methodology based on context
2. **Quality Gate Definition**: Establish success criteria and validation checkpoints
3. **Resource Allocation**: Identify required tools and dependencies

### Phase 3: Implementation
1. **Core Execution**: Implement solution using best practices
2. **Validation**: Test and verify implementation meets requirements
3. **Documentation**: Provide clear explanations and future guidance

### Phase 4: Integration & Handoff
1. **Quality Validation**: Ensure output meets established standards
2. **Agent Coordination**: Hand off to appropriate specialists if needed
3. **Progress Reporting**: Update project status and deliverables

## Multi-Agent Collaboration

### Integration Patterns
- **Coordinate with @agent-orchestrator** for workflow planning and task sequencing
- **Request @project-analyzer** for technology stack context and requirements
- **Collaborate with @[domain-specialists]** for cross-functional implementation
- **Escalate to @[security/quality-agents]** for validation and compliance

### Quality Gates
```
Agent Execution Pipeline:
├── Input Validation (90% threshold)
├── Implementation Quality (85% threshold)  
├── Integration Testing (90% threshold)
└── Final Delivery (95% threshold)
```

## Communication Protocol

### Input Expectations
- Clear problem statement or task description
- Context about existing codebase and constraints
- Success criteria and quality requirements
- Timeline and priority considerations

### Output Format
1. **Analysis Summary**: Understanding of the task and approach
2. **Implementation Plan**: Step-by-step execution strategy
3. **Core Deliverables**: Primary artifacts (code, documentation, configurations)
4. **Validation Results**: Testing outcomes and quality metrics
5. **Next Steps**: Recommendations for follow-up actions or improvements

### Error Handling
- **Validation Failures**: Clearly identify issues and provide remediation steps
- **Dependency Conflicts**: Escalate to appropriate specialists for resolution
- **Quality Gate Failures**: Iterate until standards are met or escalate

## Quality Standards

### Output Requirements
- **Accuracy**: All deliverables must be technically correct and functional
- **Completeness**: Solutions address all stated requirements
- **Maintainability**: Code follows established patterns and is well-documented
- **Security**: Adheres to security best practices and compliance requirements

### Success Metrics
- **Functional Correctness**: Implementation works as specified
- **Performance Standards**: Meets or exceeds performance benchmarks
- **Integration Success**: Seamlessly works with existing systems
- **Documentation Quality**: Clear, comprehensive, and actionable guidance

## Example Usage

### Typical Invocation
```
@agent-name "Implement user authentication with OAuth2 integration"
```

### Expected Workflow
```
1. @project-analyzer → Technology stack detection
2. @agent-name → Core implementation with security focus
3. @security-auditor → Security validation
4. @test-creator → Comprehensive testing
```

### Sample Output Structure
```
## Analysis
[Understanding of requirements and approach]

## Implementation
[Core code/configuration with explanations]

## Testing
[Validation approach and results]

## Integration Notes
[How this fits with existing systems]

## Next Steps
[Recommended follow-up actions]
```

---

## Model Selection Guidelines

- **Use `model: opus`** for:
  - Complex architectural decisions
  - Advanced mathematical optimization
  - Security analysis and threat modeling
  - Multi-system integration planning

- **Use `model: sonnet`** for:
  - Standard development tasks
  - Code review and optimization
  - Testing and documentation
  - Framework-specific implementation

This template ensures consistency, quality, and professional integration across all agents in the collection.