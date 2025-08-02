# Claude Code Sub-Agents Collection

A meticulously crafted collection of 140+ specialized Claude Code sub-agents designed for accuracy, efficiency, and comprehensive software development support.

## 🎯 Overview

This collection provides specialized AI sub-agents for every aspect of software development, from coding and testing to DevOps and specialized domains. Each sub-agent is optimized for specific tasks with detailed system prompts, tool configurations, and usage examples.

## 📁 Structure

```
subagents/
├── development-programming/     # 48 agents - Core development tasks
├── testing-qa/                 # 18 agents - Testing and quality assurance
├── devops-infrastructure/       # 2 agents - DevOps and infrastructure
├── security-compliance/         # 11 agents - Security and compliance
├── data-analytics/             # 3 agents - Data and analytics
└── README.md                   # Detailed documentation
```

## 🚀 Quick Start

### Using Sub-Agents in Claude Code

1. **Create a sub-agent** using the `/agents` command:
   ```
   /agents create code-reviewer-pro
   ```

2. **Configure with provided system prompt** from the corresponding markdown file

3. **Assign appropriate tools** based on the agent's requirements

4. **Invoke the sub-agent** for specialized tasks

### Example Usage

```
# Create and configure the Code Reviewer Pro sub-agent
/agents create code-reviewer-pro

# Use the sub-agent for code review
@code-reviewer-pro Review this authentication module for security issues
```

## 📋 Available Categories

- **Development & Programming** (48 agents) - Core development specialists
- **Testing & Quality Assurance** (18 agents) - Comprehensive testing specialists
- **DevOps & Infrastructure** (2 agents) - Infrastructure and deployment specialists
- **Security & Compliance** (11 agents) - Security-focused specialists
- **Data & Analytics** (3 agents) - Data engineering and analytics specialists

## 📖 Documentation

For detailed information about each category and individual agents, see the [comprehensive documentation](./subagents/README.md).

## 🛠️ Configuration

Each sub-agent includes:
- Detailed system prompt with expertise areas
- Required tool configurations
- Usage examples and best practices
- Specific specialization areas

## 📝 License

This collection is designed for use with Claude Code and follows best practices for AI-assisted software development.

---

*Each sub-agent represents specialized knowledge and best practices in its domain, providing expert-level assistance for software development tasks.*