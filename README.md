# Claude Code Commander 🤖

> A comprehensive collection of specialized Claude Code agents that transform your AI assistant into an expert development team.

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Sub--Agents-blue)](https://docs.anthropic.com/en/docs/claude-code/sub-agents)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agents](https://img.shields.io/badge/Agents-8-green)]()

## 🚀 What is Claude Code Commander?

Claude Code Commander provides battle-tested agent templates that extend Claude Code's capabilities through specialized sub-agents. Each agent is an expert in their domain, allowing you to tackle complex development tasks with the right specialist for the job.

### Why Use Sub-Agents?

- **🧠 Specialized Expertise**: Each agent masters specific domains with deep knowledge
- **⚡ Parallel Execution**: Run up to 10 agents simultaneously for faster results
- **🔒 Context Isolation**: Agents maintain separate memory to prevent confusion
- **🎯 Auto-Triggering**: Agents activate automatically based on your requests
- **🛠️ Tool Control**: Fine-grained permissions for each agent's capabilities

## 📦 Quick Start

Before you install the agents, the Cloudflare Agent assumes it will have access to the Cloudflare Docs MCP

Cloudflare Docs
Just run the following command from your command line and claude will install it. 

claude mcp add --transport sse cloudflare-docs -s user https://docs.mcp.cloudflare.com/sse

More info about Cloudflare mcps here
https://developers.cloudflare.com/agents/model-context-protocol/mcp-servers-for-cloudflare/


##Claude Code can install the agents for you:

### 0. Tell Claude Code
install the claude code sub agents from 
https://github.com/jezweb/claude-code-commander

<img width="780" height="1384" alt="claude-code-commander-01" src="https://github.com/user-attachments/assets/89044d0f-8866-45aa-aa5b-9177ee365ead" />


### Once installation is complete, you need to exit claude code and then re-open it!

<img width="723" height="814" alt="claude-code-commander-02" src="https://github.com/user-attachments/assets/10de9eda-4e5d-4fe1-9b5a-a4d9cdfcc531" />


## Manual Installation

### 1. Clone the Repository

```bash
git clone https://github.com/jezweb/claude-code-commander.git
cd claude-code-commander
```

### 2. Install Agents

#### Option A: Install All Agents (Recommended)
```bash
# Copy all agents to your Claude Code configuration
cp -r agent-templates/* ~/.claude/agents/
```

#### Option B: Install Specific Agents
```bash
# Install only the agents you need
cp agent-templates/code-reviewer.md ~/.claude/agents/
cp agent-templates/cloudflare-developer.md ~/.claude/agents/
```

### 3. Verify Installation

In Claude Code, type:
```
/agents
```

You should see all installed agents listed.

### 4. Start Using Agents

Simply describe what you need, and the appropriate agent will activate:

```
"Review my code for security vulnerabilities"
"Deploy my app to Cloudflare Workers"
"Write comprehensive tests for the shopping cart"
```

## 🗂️ Repository Structure

```
claude-code-commander/
├── README.md                           # This file
├── claude-code-agents-guide.md         # Comprehensive guide to sub-agents
├── agent-templates/                    # Ready-to-use agent templates
│   ├── code-reviewer.md               # Senior code review specialist
│   ├── test-runner.md                 # TDD and testing expert
│   ├── documentation-expert.md        # Technical documentation writer
│   ├── orchestrator.md                # Project coordination lead
│   ├── debugger.md                    # Advanced debugging specialist
│   ├── cloudflare-developer.md        # Cloudflare deployment expert
│   ├── railway-developer.md           # Railway container deployment specialist
│   └── mcp-developer.md               # Model Context Protocol specialist
└── examples/                          # Practical workflow examples
    ├── parallel-analysis.md           # Multi-agent codebase analysis
    ├── competing-solutions.md         # Architecture comparison patterns
    ├── tdd-workflow.md                # Test-driven development flows
    └── team-setup.md                  # AI team configuration
```

## 🤖 Available Agents

### 1. 🔍 [Code Reviewer](agent-templates/code-reviewer-README.md)
**Senior Code Review Specialist**
- 15+ years of experience across multiple languages
- Security vulnerability detection (OWASP Top 10)
- Performance optimization analysis
- Architecture and design feedback

**Triggers**: `"review code"`, `"check security"`, `"code feedback"`

### 2. 🧪 [Test Runner](agent-templates/test-runner-README.md)
**Test-Driven Development Expert**
- Comprehensive test suite creation
- Multiple framework support (Jest, pytest, JUnit, etc.)
- TDD workflow implementation
- Coverage analysis and improvement

**Triggers**: `"write tests"`, `"implement TDD"`, `"test coverage"`

### 3. 📚 [Documentation Expert](agent-templates/documentation-expert-README.md)
**Technical Documentation Specialist**
- README and API documentation
- Architecture documentation with diagrams
- User guides and tutorials
- Migration and troubleshooting guides

**Triggers**: `"document"`, `"write README"`, `"API docs"`

### 4. 🎯 [Orchestrator](agent-templates/orchestrator-README.md)
**Technical Lead Coordinator**
- Complex project breakdown
- Multi-agent task coordination
- Three-phase execution model
- Risk assessment and mitigation

**Triggers**: `"coordinate development"`, `"plan architecture"`, `"orchestrate"`

### 5. 🐛 [Debugger](agent-templates/debugger-README.md)
**Advanced Debugging Specialist**
- Systematic root cause analysis
- Platform-specific debugging
- Performance profiling
- Memory leak detection

**Triggers**: `"debug"`, `"fix error"`, `"investigate issue"`

### 6. ☁️ [Cloudflare Developer](agent-templates/cloudflare-developer-README.md)
**Cloudflare Platform Expert**
- Workers, Pages, and R2 deployment
- Workers AI integration (50+ models)
- Wrangler CLI mastery
- Edge computing optimization

**Triggers**: `"deploy to cloudflare"`, `"wrangler"`, `"workers AI"`

### 7. 🚂 [Railway Developer](agent-templates/railway-developer-README.md)
**Railway Container Deployment Specialist**
- Docker container optimization
- Railway CLI command expertise
- PostgreSQL and Redis integration
- CI/CD pipeline configuration

**Triggers**: `"deploy to railway"`, `"railway up"`, `"docker container"`

### 8. 🔌 [MCP Developer](agent-templates/mcp-developer-README.md)
**Model Context Protocol Specialist**
- 2025 MCP standards (Streamable HTTP, OAuth 2.1)
- Cloudflare McpAgent deployment with Durable Objects
- Docker container MCP Catalog integration
- Multi-client configuration (Claude Desktop, Cursor, VSCode)

**Triggers**: `"build MCP server"`, `"MCP authentication"`, `"streamable HTTP"`

## 💡 Usage Examples

### Simple Agent Invocation
```
"Review this code for security issues and performance"
```

### Parallel Multi-Agent Analysis
```
"Analyze this codebase using 4 parallel agents:
1. Architecture review
2. Security audit  
3. Performance analysis
4. Documentation check"
```

### Orchestrated Development
```
"Build a complete user authentication system with tests and documentation"
```

The orchestrator will coordinate:
- Backend developer for API
- Frontend developer for UI
- Test engineer for test suite
- Documentation expert for guides

## 🛠️ Creating Custom Agents

### Agent Template Structure

```markdown
---
name: agent-name
description: When to use this agent (specific triggers)
tools: read, write, edit, bash, grep  # Allowed tools
color: agent-name  # Visual identifier
---

You are an expert in [domain]. Your role is to [primary responsibility].

## Core Competencies
- [Competency 1]
- [Competency 2]

## Approach
[How the agent works]

## Examples
[Specific examples of agent behavior]
```

### Adding Your Agent

1. Create your agent file in `agent-templates/`
2. Follow the naming convention: `domain-role.md`
3. Test locally: `cp your-agent.md ~/.claude/agents/`
4. Submit a PR with your agent and README

## 📖 Documentation

- **[Complete Guide](claude-code-agents-guide.md)** - Comprehensive framework for building agents
- **[Examples](examples/)** - Real-world workflow patterns
- **[Agent Templates](agent-templates/)** - Ready-to-use specialized agents

## 🤝 Contributing

We welcome contributions! To add your own agents:

1. Fork the repository
2. Create your agent following our templates
3. Add a README for your agent
4. Test thoroughly
5. Submit a pull request

Please ensure your agent:
- Has a clear, specific purpose
- Includes comprehensive documentation
- Follows the established template structure
- Includes real-world examples

## 🌟 Best Practices

1. **Start Simple**: Use one agent at a time initially
2. **Clear Instructions**: Be specific about what you need
3. **Leverage Parallelism**: Use multiple agents for complex tasks
4. **Trust the Experts**: Let agents work in their specialization
5. **Iterate**: Refine agent prompts based on results

## 🚧 Troubleshooting

### Agent Not Triggering
- Check if agent is installed: `/agents`
- Verify trigger phrases match description
- Try explicit invocation: `"Use code-reviewer agent to..."`

### Performance Issues
- Reduce number of parallel agents
- Break complex tasks into phases
- Clear context between major operations

### Unexpected Behavior
- Check agent has required tool access
- Verify agent prompt hasn't been modified
- Review examples in agent README

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Anthropic](https://anthropic.com) for Claude Code and sub-agents feature
- Community contributors and agent creators
- Inspired by various GitHub agent collections

## 📬 Support

- **Issues**: [GitHub Issues](https://github.com/jezweb/claude-code-commander/issues)
- **Discussions**: [GitHub Discussions](https://github.com/jezweb/claude-code-commander/discussions)
- **Claude Community**: [Discord #claude-code](https://discord.gg/anthropic)

---

**Ready to command your AI development team?** Install the agents and start building! 🚀
