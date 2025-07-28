# The Complete Claude Code Sub-Agents Guide

> A comprehensive framework for building effective Claude Code sub-agents based on community learnings, best practices, and real-world implementations.

## Table of Contents

1. [Introduction](#introduction)
2. [Core Concepts](#core-concepts)
3. [Creating Agents](#creating-agents)
4. [Agent Anatomy](#agent-anatomy)
5. [Best Practices](#best-practices)
6. [Advanced Techniques](#advanced-techniques)
7. [Real-World Patterns](#real-world-patterns)
8. [Community Resources](#community-resources)
9. [Troubleshooting](#troubleshooting)
10. [Future Directions](#future-directions)

## Introduction

Claude Code sub-agents are specialized AI assistants that extend Claude Code's capabilities by operating with their own context windows, custom system prompts, and specific tool permissions. Launched in late 2024, this feature has transformed Claude Code from a single assistant into an orchestratable AI development team.

### Why Sub-Agents Matter

- **Context Isolation**: Each agent maintains its own memory, preventing context pollution
- **Parallel Execution**: Up to 10 agents can run simultaneously (100 can be queued)
- **Specialized Expertise**: Agents can be fine-tuned for specific domains
- **Team Collaboration**: Mimics real development team dynamics

## Core Concepts

### What Are Sub-Agents?

Sub-agents are lightweight instances of Claude Code that:
- Run inside tasks with separate context windows
- Can be configured with custom system prompts
- Have controllable tool access
- Execute in parallel or sequentially
- Report results back to the main agent

### Key Characteristics

1. **Independence**: Each agent operates in its own context
2. **Specialization**: Focused on specific tasks or domains
3. **Coordination**: Can be orchestrated by a main agent
4. **Tool Access**: Inherits or restricts tools as needed
5. **Persistence**: Project or user-level storage

## Creating Agents

### Method 1: Interactive Creation

```bash
# Launch Claude Code and use the agents command
/agents

# Follow the interactive prompts:
# 1. Select "Create new agent"
# 2. Choose scope (project or user)
# 3. Name your agent
# 4. Add description
# 5. Configure tool access
# 6. Edit system prompt
```

### Method 2: Manual Creation

Create agent files in the appropriate directory:

```bash
# Project-level agents
mkdir -p .claude/agents

# User-level agents
mkdir -p ~/.claude/agents
```

### Basic Agent Template

```markdown
---
name: agent-name
description: When to use this agent (be specific for auto-invocation)
tools: read, edit, bash  # Optional - inherits all if omitted
color: agent-name  # Optional visual identifier
---

You are an expert in [domain]. Your role is to [primary responsibility].

## Core Competencies
- [Competency 1]
- [Competency 2]
- [Competency 3]

## Guidelines
1. [Guideline 1]
2. [Guideline 2]
3. [Guideline 3]

## Examples
[Provide specific examples of expected behavior]
```

## Agent Anatomy

### Required Components

1. **Name**: Unique identifier (kebab-case recommended)
2. **Description**: Clear trigger conditions for auto-invocation
3. **System Prompt**: Detailed instructions and personality

### Optional Components

1. **Tools**: Comma-separated list of allowed tools
2. **Color**: Visual identifier in Claude Code
3. **Examples**: Improve agent understanding

### Tool Access Options

Common tools agents can access:
- `read` - Read files
- `edit` - Edit files
- `write` - Create files
- `bash` - Execute commands
- `grep` - Search files
- `glob` - Find files
- `ls` - List directories
- `task` - Spawn sub-agents (restricted)
- MCP server tools (if connected)

## Best Practices

### 1. Design Principles

**Single Responsibility**
```markdown
# Good: Focused agent
name: api-endpoint-creator
description: Use when creating new REST API endpoints

# Bad: Too broad
name: backend-developer
description: Use for any backend task
```

**Clear Triggers**
```markdown
description: |
  Use this agent when:
  - Creating new database migrations
  - Modifying schema files
  - Updating ORM models
  
  Examples:
  - "Create a migration for user profiles"
  - "Add a new field to the products table"
```

### 2. System Prompt Structure

```markdown
## Identity
You are a [role] specializing in [domain].

## Mission
Your primary goal is to [main objective].

## Expertise
- Deep knowledge of [technology/framework]
- Proficiency in [specific skills]
- Understanding of [domain patterns]

## Approach
1. First, analyze [what to analyze]
2. Then, implement [how to implement]
3. Finally, validate [validation steps]

## Constraints
- Always [constraint 1]
- Never [constraint 2]
- Prefer [preference] over [alternative]

## Output Format
[Specify expected output structure]
```

### 3. Tool Management

**Minimal Access Principle**
```yaml
# Only grant necessary tools
tools: read, edit  # For code review agent
tools: bash, read  # For test runner agent
tools: write, edit, read  # For code generator agent
```

**Security Considerations**
- Limit `bash` access for untrusted operations
- Avoid granting `task` to prevent recursive spawning
- Be cautious with MCP server tool access

### 4. Naming Conventions

```
Pattern: [domain]-[action]-[modifier]

Examples:
- django-api-developer
- react-component-architect
- security-vulnerability-scanner
- performance-optimizer
```

## Advanced Techniques

### 1. Thinking Modes

Leverage Claude's thinking capabilities:

```markdown
When facing complex problems, use progressive thinking:
- Simple issues: think
- Moderate complexity: think hard
- Complex problems: think harder
- Critical decisions: ultrathink
```

### 2. Agent Orchestration

**Three-Phase Pattern**
```markdown
name: tech-lead-orchestrator
---
## Orchestration Strategy

### Phase 1: Research (Parallel)
- Spawn analysts to gather information
- Each explores different aspects
- Collect all findings

### Phase 2: Planning
- Synthesize research results
- Create task breakdown with TodoWrite
- Identify dependencies

### Phase 3: Execution (Coordinated)
- Assign tasks to specialists
- Monitor progress
- Handle blockers
```

### 3. Context Window Management

```markdown
## Memory Optimization
- Summarize findings before returning
- Focus on actionable insights
- Avoid verbose explanations
- Return structured data when possible
```

### 4. Inter-Agent Communication

```markdown
## Handoff Protocol
When delegating to another agent:
1. Summarize current context
2. Specify exact requirements
3. Define expected output format
4. Set clear success criteria
```

## Real-World Patterns

### 1. Parallel Analysis Pattern

```markdown
"Analyze this codebase using 4 parallel agents:
- Architecture Reviewer: Focus on structure and patterns
- Security Auditor: Find vulnerabilities
- Performance Analyst: Identify bottlenecks
- Documentation Checker: Assess completeness"
```

### 2. Competing Solutions Pattern

```markdown
"Create two agents with different approaches:
Agent 1: Conservative Refactorer
- Minimal changes
- Preserve existing patterns
- Focus on safety

Agent 2: Modern Architect
- Latest best practices
- Complete restructuring
- Optimize for future"
```

### 3. Test-Driven Development Pattern

```markdown
Test Writer Agent:
- Analyzes requirements
- Writes comprehensive tests
- Defines edge cases

Implementation Agent:
- Reads test specifications
- Implements minimal code
- Ensures all tests pass
```

### 4. Review Pipeline Pattern

```markdown
Sequential review process:
1. Syntax Checker → Basic validation
2. Logic Reviewer → Business logic
3. Security Auditor → Vulnerability scan
4. Performance Optimizer → Efficiency improvements
```

## Community Resources

### Major Collections

1. **vijaythecoder/awesome-claude-agents**
   - 26 specialized agents
   - Complete AI dev team
   - Auto-configuration tools

2. **atournayre/claude-agents**
   - PHP/Symfony focused
   - DDD patterns
   - 15+ professional agents

3. **hesreallyhim/awesome-claude-code-agents**
   - Growing collection
   - Community contributions
   - Framework examples

### Notable Individual Agents

**Code Reviewer**
```markdown
name: senior-code-reviewer
description: Proactive code review after significant changes
---
You are a senior engineer with 15+ years experience...
[Full implementation in agent-templates/]
```

**MCP Protocol Expert**
```markdown
name: mcp-protocol-expert
description: Model Context Protocol development assistance
---
You are an elite MCP expert...
[Handles client/server implementation]
```

### Community Tools

1. **Claude Code Templates** - Project setup automation
2. **CC Usage** - Token usage analytics
3. **Claude Code WebUI** - Web interface
4. **Code By Agents** - Orchestration framework

## Troubleshooting

### Common Issues

**1. Agent Not Triggering**
- Check description specificity
- Verify agent file location
- Ensure proper YAML formatting
- Test with explicit invocation

**2. Context Overflow**
```markdown
## Solutions:
- Use summarization in prompts
- Limit tool output verbosity
- Clear context between phases
- Batch similar operations
```

**3. Tool Access Errors**
- Verify tool names are correct
- Check for typos in tools list
- Ensure MCP servers are connected
- Test with minimal tool set

### Debugging Techniques

```bash
# List all available agents
/agents

# Test specific agent
"Use the [agent-name] agent to [task]"

# Check agent loading
ls .claude/agents/
ls ~/.claude/agents/
```

### Performance Optimization

1. **Parallel Execution**
   ```
   # Good: Parallel tasks
   "Analyze these 5 files using separate agents"
   
   # Avoid: Sequential when parallel possible
   "Analyze file1, then file2, then file3..."
   ```

2. **Tool Usage**
   ```markdown
   ## Efficient Patterns
   - Batch file reads
   - Use glob before read
   - Minimize bash calls
   - Cache common results
   ```

## Future Directions

### Emerging Patterns

1. **Self-Improving Agents**: Agents that update their own prompts
2. **Agent Mesh Networks**: Complex inter-agent communication
3. **Domain-Specific Languages**: Custom DSLs for agent definition
4. **Learning Agents**: Agents that adapt based on usage

### Community Trends

- Standardized agent libraries
- Cross-project agent sharing
- Agent marketplace concepts
- Visual orchestration tools

### Best Practices Evolution

As the community grows, expect:
- Formalized agent design patterns
- Testing frameworks for agents
- Agent composition tools
- Performance benchmarking

## Conclusion

Claude Code sub-agents represent a paradigm shift in AI-assisted development. By understanding these patterns and best practices, developers can build sophisticated AI teams that dramatically improve productivity and code quality.

### Key Takeaways

1. **Start Simple**: Use existing agents before creating custom ones
2. **Think in Teams**: Design agents that work together
3. **Iterate Often**: Refine agents based on usage
4. **Share Knowledge**: Contribute to community collections
5. **Stay Updated**: This space is evolving rapidly

### Next Steps

1. Install a community agent collection
2. Create your first custom agent
3. Experiment with orchestration patterns
4. Share your successful agents
5. Join the community discussions

---

*This guide is a living document. As Claude Code and its agent system evolve, so too will these practices and patterns. Stay connected with the community for the latest developments.*

**Last Updated**: January 2025  
**Based on**: Community research and real-world implementations  
**Contributions**: Welcome via GitHub