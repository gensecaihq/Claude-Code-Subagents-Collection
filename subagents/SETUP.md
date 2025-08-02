# Claude Code Sub-Agents Setup Guide

Complete installation and configuration guide for the specialized sub-agents collection.

## 📋 Prerequisites

- Claude Code CLI installed and configured
- Access to Claude API
- Basic understanding of Claude Code sub-agents
- Project repository (recommended)

## 🚀 Quick Setup

### 1. Verify Claude Code Installation

```bash
# Check Claude Code version
claude --version

# Verify sub-agent support
claude /help agents
```

### 2. Initialize Sub-Agents in Your Project

```bash
# Navigate to your project
cd your-project

# Create .claude directory if it doesn't exist
mkdir -p .claude

# Copy sub-agent configurations
cp -r path/to/subagents .claude/subagents
```

### 3. Create Your First Sub-Agent

```bash
# Start Claude Code
claude

# Create a sub-agent using the /agents command
/agents create code-reviewer-pro

# Configure the agent with the provided system prompt
# (Copy from subagents/development-programming/code-reviewer-pro.md)
```

## 🛠️ Detailed Configuration

### Agent Creation Workflow

1. **Choose the Right Agent**
   - Review the [README.md](README.md) for agent descriptions
   - Identify the specialist that matches your task
   - Check the agent's tool requirements

2. **Create the Sub-Agent**
   ```bash
   /agents create [agent-name]
   ```

3. **Configure System Prompt**
   - Copy the system prompt from the agent's markdown file
   - Paste into the agent configuration dialog
   - Adjust for your specific needs

4. **Set Tool Permissions**
   - Grant only necessary tools for security
   - Start with minimal permissions
   - Add tools as needed

5. **Test the Agent**
   - Start with a simple task
   - Verify the agent responds appropriately
   - Refine configuration if needed

### Tool Configuration Guide

#### Essential Tools (Recommended for All Agents)
```yaml
tools:
  - Read          # File reading capabilities
  - Write         # File creation (use sparingly)
  - Edit          # File modification
  - MultiEdit     # Multiple file changes
```

#### Analysis Tools
```yaml
analysis_tools:
  - Grep          # Pattern searching in files
  - Glob          # File pattern matching
  - Task          # Complex task delegation
```

#### Execution Tools (Use with Caution)
```yaml
execution_tools:
  - Bash          # Command execution
  - WebSearch     # Internet research
  - WebFetch      # API calls and web content
```

#### Specialized Tools
```yaml
specialized_tools:
  - NotebookRead      # Jupyter notebook reading
  - NotebookEdit      # Jupyter notebook editing
  - TodoWrite         # Task management
```

### Security Configuration

#### Tool Permission Matrix

| Agent Category | Read | Write | Edit | Bash | WebSearch | WebFetch |
|----------------|------|-------|------|------|-----------|----------|
| Code Review    | ✓    | ✗     | ✓    | ✗    | ✓         | ✗        |
| Code Creation  | ✓    | ✓     | ✓    | ✗    | ✓         | ✗        |
| Testing        | ✓    | ✓     | ✓    | ✓    | ✓         | ✗        |
| DevOps         | ✓    | ✓     | ✓    | ✓    | ✓         | ✓        |
| Security       | ✓    | ✗     | ✗    | ✗    | ✓         | ✓        |

#### Best Practices

1. **Principle of Least Privilege**
   - Grant minimal tools initially
   - Add permissions as needed
   - Remove unused tools

2. **Production Safety**
   - Never grant Write/Bash to production environments
   - Use separate agents for production tasks
   - Implement approval workflows

3. **Audit Trail**
   - Monitor agent actions
   - Log tool usage
   - Review changes before applying

## 📁 Project Integration

### Recommended Project Structure

```
your-project/
├── .claude/
│   ├── subagents/           # Agent configurations
│   ├── settings.json        # Claude Code settings
│   └── agents.json          # Agent definitions
├── src/                     # Source code
├── tests/                   # Test files
├── docs/                    # Documentation
└── README.md
```

### Team Configuration

#### Shared Agent Configurations

Create a team configuration file:

```json
{
  "team_agents": {
    "code_reviewer": {
      "name": "Code Reviewer Pro",
      "system_prompt_file": ".claude/subagents/development-programming/code-reviewer-pro.md",
      "tools": ["Read", "Edit", "Grep", "WebSearch"],
      "description": "Security-focused code review specialist"
    },
    "test_creator": {
      "name": "Test Creator",
      "system_prompt_file": ".claude/subagents/testing-qa/test-creator.md", 
      "tools": ["Read", "Write", "Edit", "Bash"],
      "description": "Comprehensive test case generator"
    }
  }
}
```

#### Environment-Specific Agents

```json
{
  "environments": {
    "development": {
      "agents": ["code-reviewer", "test-creator", "debugger"],
      "additional_tools": ["Bash", "Write"]
    },
    "staging": {
      "agents": ["performance-tester", "integration-tester"],
      "additional_tools": ["WebFetch", "Bash"]
    },
    "production": {
      "agents": ["security-auditor", "monitor"],
      "additional_tools": ["Read", "WebSearch"]
    }
  }
}
```

## 🔧 Advanced Configuration

### Custom Agent Creation

1. **Create Agent Definition**
   ```markdown
   # Custom Agent Name
   
   ## Description
   Your custom agent description
   
   ## System Prompt
   Your custom system prompt based on needs
   
   ## Tools
   Required tools for your agent
   
   ## Usage Example
   How to use your custom agent
   ```

2. **Register Agent**
   ```bash
   /agents create custom-agent-name
   # Configure with your custom prompt
   ```

### Agent Composition Patterns

#### Sequential Processing
```bash
# Chain agents for complex workflows
@questioner "Clarify requirements for the user authentication system"
@architect "Design system architecture based on clarified requirements"  
@create "Implement the designed authentication system"
@test-creator "Generate comprehensive tests for the implementation"
@code-reviewer-pro "Review the complete implementation for security"
```

#### Parallel Analysis
```bash
# Use multiple agents for different perspectives
@code-reviewer-pro "Review for code quality and security"
@performance-optimizer "Analyze for performance bottlenecks" 
@accessibility-tester "Check for accessibility compliance"
```

#### Iterative Refinement
```bash
# Progressive improvement workflow
@improve "Suggest improvements to this module"
@refactoring-specialist "Implement the suggested improvements"
@test-creator "Update tests for refactored code"
@code-reviewer-pro "Validate the improvements"
```

## 🚨 Troubleshooting

### Common Issues

#### Agent Not Responding
1. Check tool permissions
2. Verify system prompt configuration
3. Ensure agent has access to relevant files
4. Review Claude Code logs

#### Tool Permission Errors
1. Grant required tools in agent configuration
2. Check project-level tool restrictions
3. Verify user permissions

#### Poor Agent Performance
1. Refine system prompt for clarity
2. Provide better context
3. Use more specific agents
4. Break down complex tasks

### Debug Commands

```bash
# List all agents
/agents list

# Show agent configuration
/agents show [agent-name]

# Test agent with simple task
@[agent-name] "Simple test task"

# Check tool permissions
/settings tools
```

## 📊 Monitoring and Metrics

### Usage Tracking

Monitor agent effectiveness:

1. **Task Completion Rate**
   - Track successful task completion
   - Identify failing patterns
   - Measure time to completion

2. **Quality Metrics**
   - Code quality improvements
   - Bug detection rate
   - Test coverage increases

3. **Team Productivity**
   - Time saved on routine tasks
   - Developer satisfaction
   - Knowledge transfer efficiency

### Performance Optimization

1. **Agent Selection**
   - Use most specialized agent for task
   - Avoid over-general agents
   - Monitor agent response times

2. **Context Management**
   - Provide relevant context
   - Avoid information overload
   - Use incremental approaches

3. **Tool Efficiency**
   - Minimize tool overhead
   - Use appropriate tool combinations
   - Monitor resource usage

## 🔄 Maintenance

### Regular Updates

1. **Agent Refinement**
   - Update system prompts based on usage
   - Adjust tool permissions
   - Incorporate team feedback

2. **New Agent Addition**
   - Identify gaps in coverage
   - Create specialized agents for new needs
   - Share successful configurations

3. **Performance Review**
   - Analyze agent effectiveness
   - Remove unused agents
   - Optimize frequently used agents

### Backup and Recovery

```bash
# Backup agent configurations
cp -r .claude/subagents .claude/subagents.backup

# Export agent settings
/agents export > agents-backup.json

# Restore from backup
/agents import < agents-backup.json
```

## 🎯 Best Practices Summary

1. **Start Small**: Begin with 3-5 core agents
2. **Security First**: Grant minimal tools initially
3. **Team Alignment**: Standardize configurations across team
4. **Iterative Improvement**: Refine agents based on usage
5. **Documentation**: Document custom configurations
6. **Monitor Usage**: Track effectiveness and adjust
7. **Regular Review**: Update agents as needs evolve

## 📞 Support

For issues and questions:

1. Review this setup guide
2. Check the main [README.md](README.md)
3. Consult Claude Code documentation
4. Reach out to your team's Claude Code admin

---

*This setup guide ensures you get maximum value from the specialized sub-agents while maintaining security and team collaboration.*