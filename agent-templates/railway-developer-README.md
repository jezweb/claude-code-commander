# Railway Container Developer Agent 🚂

> A Docker and Railway deployment specialist that helps you containerize applications and deploy them seamlessly to Railway's infrastructure platform.

## Overview

The Railway Container Developer agent specializes in:
- **Railway CLI**: Complete command mastery for deployments and management
- **Docker Optimization**: Multi-stage builds and container best practices
- **Database Integration**: PostgreSQL, MySQL, and Redis deployment
- **Build Systems**: Nixpacks automatic builds and custom Dockerfiles
- **CI/CD Pipelines**: GitHub Actions and GitLab CI integration
- **Cost Optimization**: Usage-based billing strategies

## When to Use

This agent automatically activates when you:
- Deploy applications to Railway
- Work with Railway CLI commands
- Need Docker container optimization
- Set up databases on Railway
- Configure CI/CD for Railway deployments
- Troubleshoot Railway services

### Trigger Phrases
- `"deploy to railway"`
- `"railway up"`
- `"docker container"`
- `"railway postgres"`
- `"railway redis"`
- `"nixpacks"`

## Example Usage

### Basic Deployment
```
"Deploy this Node.js app to Railway with PostgreSQL"
```

### Docker Optimization
```
"Optimize my Dockerfile for Railway deployment"
```

### Database Setup
```
"Set up PostgreSQL with connection pooling on Railway"
```

### CI/CD Pipeline
```
"Create GitHub Actions workflow for Railway deployment"
```

## Key Capabilities

### Railway CLI Expertise

The agent knows all Railway CLI commands:

```bash
# Primary deployment command (NOT 'railway deploy')
railway up

# Environment management
railway environment production

# SSH into running service
railway ssh

# View logs
railway logs -f
```

### Docker Best Practices

```dockerfile
# Optimized multi-stage build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
EXPOSE $PORT
CMD ["node", "dist/index.js"]
```

### Database Configuration

The agent provides production-ready database setups:

#### PostgreSQL with SSL
```javascript
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: { rejectUnauthorized: false },
  max: 20,
  idleTimeoutMillis: 30000
});
```

#### Redis with TLS
```javascript
const redis = createClient({
  url: process.env.REDIS_URL,
  socket: {
    tls: true,
    rejectUnauthorized: false
  }
});
```

## Deployment Workflows

### 1. Simple Web App
```bash
# Initialize and deploy
railway init my-app
railway up

# Add PostgreSQL
railway add --database=postgresql
```

### 2. Microservices
```bash
# Deploy multiple services
cd api && railway up --service=api
cd ../worker && railway up --service=worker
cd ../frontend && railway up --service=frontend
```

### 3. Full-Stack Application
The agent helps configure:
- Frontend static hosting
- Backend API service
- Database connections
- Redis caching
- Environment variables

## CI/CD Integration

### GitHub Actions Template
```yaml
name: Deploy to Railway
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to Railway
        env:
          RAILWAY_TOKEN: ${{ secrets.RAILWAY_TOKEN }}
        run: |
          npm install -g @railway/cli
          railway up
```

## Configuration Files

### railway.toml
```toml
[build]
builder = "NIXPACKS"
buildCommand = "npm run build"

[deploy]
startCommand = "npm start"
healthcheckPath = "/health"
restartPolicyType = "ALWAYS"
```

## Build Systems

### Nixpacks (Default)
- Automatic language detection
- Zero configuration for standard apps
- Support for most frameworks

### Custom Dockerfile
- Full control over build process
- Multi-stage build optimization
- Custom base images

## Cost Optimization

The agent helps optimize costs through:

1. **Memory Optimization**
   - Monitor actual usage
   - Scale based on demand
   - Efficient caching strategies

2. **CPU Usage**
   - Optimize algorithms
   - Use worker processes wisely
   - Implement request queuing

3. **Database Connections**
   - Connection pooling
   - Query optimization
   - Proper indexing

### Typical Costs
- **Hobby Project**: ~$5/month
- **Small App**: ~$12/month
- **Production App**: ~$50+/month

## Monitoring & Debugging

### Health Checks
```javascript
app.get('/health', async (req, res) => {
  const checks = {
    database: await checkDatabase(),
    redis: await checkRedis(),
    memory: process.memoryUsage()
  };
  res.json({ status: 'healthy', checks });
});
```

### SSH Debugging
```bash
# Connect to running container
railway ssh

# Inside container
ps aux          # Check processes
env | grep RAILWAY  # View environment
df -h           # Check disk space
```

## Best Practices

1. **Environment Variables**
   - Never hardcode secrets
   - Use Railway's variable management
   - Validate required variables on startup

2. **Port Configuration**
   - Always use `process.env.PORT`
   - Never hardcode port numbers
   - Railway assigns ports automatically

3. **Graceful Shutdown**
   - Handle SIGTERM signals
   - Close database connections
   - Finish pending requests

4. **Logging**
   - Use structured logging
   - Include request context
   - Monitor error rates

## Common Issues & Solutions

### Build Failures
```bash
# Force rebuild
railway up --no-cache

# Check build logs
railway logs --build
```

### Connection Issues
- Implement retry logic
- Use connection pooling
- Check SSL/TLS settings

### Performance Problems
- Monitor resource usage
- Optimize container size
- Use caching effectively

## Advanced Features

### Private Networking
Services communicate internally using:
```
http://[service-name].[project-name].internal:[port]
```

### Pre-Deploy Commands
Handle migrations and setup:
```toml
[deploy]
preDeploy = "npm run migrate"
```

### Multi-Environment Setup
- Production, staging, development
- Environment-specific configurations
- Isolated databases per environment

## Tips for Success

1. **Start Simple**: Deploy a basic app first
2. **Use Templates**: Railway provides many starter templates
3. **Monitor Usage**: Check dashboard regularly
4. **Optimize Gradually**: Profile before optimizing
5. **Leverage Community**: Railway has an active Discord

---

**Remember**: Railway simplifies deployment without sacrificing flexibility. The agent helps you leverage Railway's platform effectively while following Docker and deployment best practices!