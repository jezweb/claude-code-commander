# AI Development Team Setup Example

This example shows how to set up a complete AI development team using Claude Code agents, with auto-configuration and team coordination patterns.

## Quick Team Setup

### Step 1: Auto-Configuration Command

```bash
"Use team-configurator to set up my AI development team for this project"
```

The team configurator will:
1. Analyze your project structure
2. Detect technology stack
3. Create appropriate agent mappings
4. Update CLAUDE.md with team configuration

### Step 2: Manual Team Setup

For more control, you can manually configure your team:

```markdown
## AI Development Team Configuration

### Core Team Members

#### 1. Technical Lead
```yaml
name: tech-lead-orchestrator
role: Project coordination and architecture decisions
triggers:
  - "plan the implementation"
  - "coordinate development"
  - "design architecture"
```

#### 2. Backend Specialist
```yaml
name: backend-developer
role: Server-side development and API design
triggers:
  - "implement backend"
  - "create API endpoint"
  - "database operations"
```

#### 3. Frontend Specialist
```yaml
name: frontend-developer
role: UI/UX implementation and client-side logic
triggers:
  - "build UI"
  - "create component"
  - "implement frontend"
```

#### 4. Database Expert
```yaml
name: database-architect
role: Schema design and query optimization
triggers:
  - "design database"
  - "optimize query"
  - "migration strategy"
```

#### 5. Test Engineer
```yaml
name: test-driven-developer
role: Test creation and quality assurance
triggers:
  - "write tests"
  - "test coverage"
  - "implement TDD"
```

#### 6. Security Auditor
```yaml
name: security-specialist
role: Security reviews and vulnerability assessment
triggers:
  - "security review"
  - "check vulnerabilities"
  - "implement auth"
```

#### 7. DevOps Engineer
```yaml
name: devops-specialist
role: CI/CD and infrastructure
triggers:
  - "setup deployment"
  - "configure CI/CD"
  - "containerize"
```

#### 8. Documentation Writer
```yaml
name: documentation-specialist
role: Technical documentation and guides
triggers:
  - "document"
  - "write README"
  - "API docs"
```
```

## Project-Specific Team Examples

### 1. React + Node.js Team

```bash
mkdir -p .claude/agents
```

Create `.claude/CLAUDE.md`:
```markdown
# Project: E-Commerce Platform
Tech Stack: React, Node.js, PostgreSQL, Redis

## AI Team Assignments

### Frontend Team
- **Lead**: react-component-architect
- **Support**: css-styling-expert, accessibility-specialist

### Backend Team  
- **Lead**: node-api-developer
- **Support**: express-middleware-expert, jwt-auth-specialist

### Data Team
- **Lead**: postgresql-expert
- **Support**: redis-cache-specialist, migration-manager

### Quality Team
- **Lead**: jest-testing-expert
- **Support**: cypress-e2e-tester, performance-auditor

## Workflow Rules
1. All features start with tech-lead-orchestrator
2. Frontend and backend development can be parallel
3. Tests must be written before implementation
4. Security review required before deployment
```

### 2. Python FastAPI Team

```markdown
# Project: ML API Service
Tech Stack: Python, FastAPI, PyTorch, Docker

## AI Team Assignments

### API Development
- **Primary**: fastapi-expert
- **Secondary**: pydantic-validator, async-python-dev

### ML Engineering
- **Primary**: pytorch-specialist  
- **Secondary**: model-optimizer, data-pipeline-engineer

### Infrastructure
- **Primary**: docker-compose-expert
- **Secondary**: kubernetes-deployer, monitoring-specialist

## Team Coordination Pattern
1. ML team develops models
2. API team wraps models
3. Infrastructure team deploys
4. Monitoring team validates
```

## Advanced Team Patterns

### 1. Microservices Team Structure

```markdown
## Microservices Development Team

### Service Teams (Per Service)
- **Service Owner**: Responsible for service design
- **API Designer**: Contract-first development
- **Implementation Dev**: Business logic
- **Test Engineer**: Service testing

### Platform Team
- **API Gateway Expert**: Request routing
- **Service Mesh Specialist**: Inter-service communication
- **Observability Engineer**: Monitoring and tracing
- **Security Architect**: Zero-trust implementation

### Coordination
Use tech-lead-orchestrator to coordinate:
"Coordinate microservices development for user service, order service, and payment service with proper API contracts and service mesh configuration"
```

### 2. Mobile App Team

```markdown
## Mobile Development Team

### iOS Team
- swift-ui-developer
- ios-architect
- core-data-specialist

### Android Team  
- kotlin-developer
- android-architect
- room-database-expert

### Shared Team
- api-integration-specialist
- mobile-security-expert
- app-store-optimizer

### Backend Team
- mobile-backend-developer
- push-notification-expert
- real-time-sync-specialist
```

## Team Communication Patterns

### 1. Daily Standup Simulation

```markdown
"Run daily standup with all active agents:
- What was completed yesterday?
- What's planned for today?
- Any blockers?"

Expected Output:
## Daily Standup - [Date]

### Frontend Team
✅ Completed: Login component with validation
📋 Today: User dashboard layout
🚫 Blockers: Need API endpoint for user data

### Backend Team
✅ Completed: Authentication endpoints
📋 Today: User profile CRUD operations
🚫 Blockers: None

### Test Team
✅ Completed: Auth endpoint tests
📋 Today: Integration tests for user flow
🚫 Blockers: Waiting for frontend components
```

### 2. Architecture Review Meeting

```markdown
"Conduct architecture review with:
- Tech Lead presenting design
- Security Auditor reviewing vulnerabilities
- Performance Expert analyzing bottlenecks
- Database Architect checking data model"
```

### 3. Sprint Planning

```markdown
"Plan next sprint with team:
1. Review backlog items
2. Estimate complexity
3. Assign to appropriate agents
4. Identify dependencies"
```

## Team Workflow Automation

### 1. Feature Development Flow

```yaml
# .claude/workflows/feature-development.yml
name: Feature Development

steps:
  - name: Requirements Analysis
    agent: tech-lead-orchestrator
    action: Analyze requirements and create task breakdown
    
  - name: Technical Design
    parallel:
      - agent: backend-architect
        action: Design API structure
      - agent: frontend-architect
        action: Design component hierarchy
      - agent: database-architect
        action: Design data model
        
  - name: Implementation
    parallel:
      - agent: backend-developer
        action: Implement API endpoints
      - agent: frontend-developer
        action: Build UI components
        
  - name: Testing
    agent: test-engineer
    action: Write and run tests
    
  - name: Review
    parallel:
      - agent: code-reviewer
        action: Code quality check
      - agent: security-auditor
        action: Security review
```

### 2. Bug Fix Flow

```yaml
# .claude/workflows/bug-fix.yml
name: Bug Fix

steps:
  - name: Investigation
    agent: debugger
    action: Reproduce and identify root cause
    
  - name: Fix Development
    agent: $specialist  # Assigned based on bug type
    action: Implement fix
    
  - name: Test Creation
    agent: test-engineer
    action: Add regression test
    
  - name: Verification
    agent: qa-engineer
    action: Verify fix and check side effects
```

## Team Performance Metrics

### 1. Velocity Tracking

```markdown
## Team Velocity Report

### This Sprint
- **Stories Completed**: 8
- **Story Points**: 34
- **Bugs Fixed**: 12
- **Tech Debt Addressed**: 3 items

### By Agent
- backend-developer: 12 points
- frontend-developer: 10 points
- test-engineer: 8 points
- devops-engineer: 4 points

### Bottlenecks
- Frontend waiting on API specs
- Tests blocked by implementation
```

### 2. Quality Metrics

```markdown
## Code Quality Dashboard

### Coverage
- Overall: 87%
- Backend: 92%
- Frontend: 81%

### Code Review
- Average review time: 2 hours
- Issues found: 23
- Critical issues: 2

### Security
- Vulnerabilities found: 0
- Security tests: 45
```

## Team Scaling Strategies

### 1. Growing the Team

```markdown
As project grows, add specialized agents:

Phase 1 (MVP):
- Generalist agents
- Basic coverage

Phase 2 (Growth):
- Add specialists
- Domain experts

Phase 3 (Scale):
- Performance experts
- Platform engineers
- SRE specialists
```

### 2. Team Rotation

```markdown
Prevent agent fatigue by rotating responsibilities:

Week 1-2: Agent A leads backend
Week 3-4: Agent B leads backend
Result: Fresh perspectives, knowledge sharing
```

## Best Practices for AI Teams

1. **Clear Roles**: Each agent should have defined responsibilities
2. **Communication Protocols**: Establish how agents share information
3. **Escalation Paths**: Define when to involve human developers
4. **Regular Retrospectives**: Review team performance
5. **Continuous Improvement**: Update agent configurations based on learnings

## Troubleshooting Team Issues

### Common Problems and Solutions

```markdown
Problem: Agents giving conflicting advice
Solution: Establish clear hierarchy and decision authority

Problem: Too much overhead in coordination
Solution: Reduce team size, increase agent autonomy

Problem: Agents missing context
Solution: Improve handoff protocols, use shared documentation

Problem: Duplicate work
Solution: Better task assignment, clear ownership
```

Remember: The goal is to create a collaborative AI team that enhances your productivity, not to replace human judgment. Use the team to handle routine tasks, explore options, and accelerate development while maintaining human oversight for critical decisions.