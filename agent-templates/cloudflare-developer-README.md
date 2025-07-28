# Cloudflare Developer Agent ☁️

> A Cloudflare platform expert that helps you build, deploy, and optimize applications on Cloudflare's global edge network, including Workers, R2, AI, and more.

## Overview

The Cloudflare Developer agent specializes in:
- **Workers & Pages**: Serverless deployment at the edge
- **R2 Storage**: S3-compatible object storage without egress fees
- **Workers AI**: 50+ AI models running on edge GPUs
- **Wrangler CLI**: Complete command mastery
- **Edge Services**: KV, Durable Objects, D1, Queues

## When to Use

This agent automatically activates when you:
- Deploy applications to Cloudflare
- Work with Workers or Pages
- Need R2 storage integration
- Implement AI features at the edge
- Configure Cloudflare services
- Optimize for global performance

### Trigger Phrases
- `"deploy to cloudflare"`
- `"wrangler"`
- `"workers"`
- `"R2 bucket"`
- `"cloudflare AI"`
- `"edge deployment"`

## Example Usage

### Basic Worker Deployment
```
"Deploy this Express app to Cloudflare Workers"
```

### R2 Integration
```
"Set up R2 storage for user uploads with CORS"
```

### AI Implementation
```
"Add image classification using Cloudflare Workers AI"
```

### Full Stack App
```
"Deploy a React app with Workers backend and R2 storage"
```

## Key Features

### Direct Documentation Access
This agent has real-time access to Cloudflare's official documentation:
- Searches for latest features and syntax
- Verifies commands and configurations
- Provides up-to-date best practices
- Troubleshoots with official guides

## Wrangler Command Reference

### Essential Commands

#### Project Setup
```bash
# Create new Worker
npx wrangler init my-worker

# Create Pages project  
npm create cloudflare@latest my-app

# Clone existing Worker
npx wrangler init --from-dash existing-worker
```

#### Development
```bash
# Local development server
npx wrangler dev

# With local persistence
npx wrangler dev --local --persist

# Test with specific environment
npx wrangler dev --env staging
```

#### Deployment
```bash
# Deploy to production
npx wrangler deploy

# Deploy with environment
npx wrangler deploy --env production

# Deploy with version
npx wrangler versions upload
npx wrangler versions deploy
```

### R2 Storage Commands

```bash
# Create bucket
npx wrangler r2 bucket create my-bucket

# Upload file
npx wrangler r2 object put my-bucket/file.pdf --file ./local.pdf

# List objects
npx wrangler r2 object list my-bucket

# Configure CORS
npx wrangler r2 bucket cors set my-bucket --file cors.json
```

### Workers AI Commands

```bash
# List available models
npx wrangler ai models

# Test model locally
npx wrangler ai run @cf/meta/llama-3.1-8b-instruct \
  --text "Explain quantum computing"
```

## Implementation Examples

### 1. Worker with R2 Storage

```javascript
export default {
  async fetch(request, env) {
    const url = new URL(request.url);
    
    if (request.method === 'PUT') {
      // Upload to R2
      const key = `uploads/${Date.now()}-${url.pathname.slice(1)}`;
      await env.MY_BUCKET.put(key, request.body);
      
      return Response.json({ 
        success: true, 
        key,
        url: `${url.origin}/${key}`
      });
    }
    
    if (request.method === 'GET') {
      // Retrieve from R2
      const key = url.pathname.slice(1);
      const object = await env.MY_BUCKET.get(key);
      
      if (!object) {
        return new Response('Not Found', { status: 404 });
      }
      
      const headers = new Headers();
      object.writeHttpMetadata(headers);
      return new Response(object.body, { headers });
    }
  }
};
```

### 2. AI-Powered API

```javascript
export default {
  async fetch(request, env) {
    const { prompt, model = '@cf/meta/llama-3.1-8b-instruct' } = 
      await request.json();
    
    // Generate AI response
    const response = await env.AI.run(model, {
      messages: [
        { role: 'system', content: 'You are a helpful assistant.' },
        { role: 'user', content: prompt }
      ]
    });
    
    // Cache response
    const cacheKey = `ai:${model}:${prompt}`;
    await env.KV.put(cacheKey, JSON.stringify(response), {
      expirationTtl: 3600 // 1 hour
    });
    
    return Response.json(response);
  }
};
```

### 3. Image Processing Pipeline

```javascript
export default {
  async fetch(request, env) {
    const formData = await request.formData();
    const image = formData.get('image');
    
    // Store original in R2
    const key = `images/${crypto.randomUUID()}`;
    await env.IMAGES.put(key, image.stream());
    
    // Classify image with AI
    const imageArray = new Uint8Array(await image.arrayBuffer());
    const classification = await env.AI.run(
      '@cf/microsoft/resnet-50',
      { image: [...imageArray] }
    );
    
    // Store metadata in KV
    await env.IMAGE_METADATA.put(key, JSON.stringify({
      uploadedAt: new Date().toISOString(),
      classification: classification.labels,
      size: image.size,
      type: image.type
    }));
    
    return Response.json({
      key,
      classification: classification.labels[0],
      confidence: classification.scores[0]
    });
  }
};
```

## Configuration Examples

### Complete wrangler.toml

```toml
name = "my-app"
main = "src/index.js"
compatibility_date = "2024-01-01"

# R2 Buckets
[[r2_buckets]]
binding = "ASSETS"
bucket_name = "my-app-assets"

[[r2_buckets]]
binding = "USER_UPLOADS"
bucket_name = "my-app-uploads"

# KV Namespaces
[[kv_namespaces]]
binding = "CACHE"
id = "abc123"

[[kv_namespaces]]
binding = "SESSIONS"
id = "def456"

# AI Binding
[ai]
binding = "AI"

# D1 Database
[[d1_databases]]
binding = "DB"
database_name = "my-app-db"
database_id = "ghi789"

# Environment Variables
[vars]
API_VERSION = "v1"
ENVIRONMENT = "production"

# Staging Environment
[env.staging]
name = "my-app-staging"
vars = { ENVIRONMENT = "staging" }

[[env.staging.r2_buckets]]
binding = "ASSETS"
bucket_name = "my-app-staging-assets"
```

## AI Model Categories

### Text Generation
```javascript
// Llama 3.1 - General purpose
await env.AI.run('@cf/meta/llama-3.1-8b-instruct', {
  messages: [{ role: 'user', content: 'Write a haiku' }]
});

// Code generation
await env.AI.run('@cf/thebloke/deepseek-coder-6.7b-instruct-awq', {
  messages: [{ role: 'user', content: 'Write a Python function' }]
});
```

### Text Analysis
```javascript
// Sentiment analysis
await env.AI.run('@cf/huggingface/distilbert-sst-2-int8', {
  text: 'This product is amazing!'
});

// Text embeddings for search
await env.AI.run('@cf/baai/bge-base-en-v1.5', {
  text: ['Document to embed']
});
```

### Image Processing
```javascript
// Image classification
await env.AI.run('@cf/microsoft/resnet-50', {
  image: [...imageArrayBuffer]
});

// Object detection
await env.AI.run('@cf/facebook/detr-resnet-50', {
  image: [...imageArrayBuffer]
});
```

## Performance Optimization

### Edge Caching
```javascript
export default {
  async fetch(request, env, ctx) {
    const cacheKey = new Request(request.url, request);
    const cache = caches.default;
    
    // Check cache
    let response = await cache.match(cacheKey);
    
    if (!response) {
      response = await generateResponse(request, env);
      response.headers.append('Cache-Control', 's-maxage=3600');
      
      // Cache at edge
      ctx.waitUntil(cache.put(cacheKey, response.clone()));
    }
    
    return response;
  }
};
```

### Request Coalescing
```javascript
const inFlight = new Map();

async function coalesceRequests(key, generator) {
  if (inFlight.has(key)) {
    return inFlight.get(key);
  }
  
  const promise = generator();
  inFlight.set(key, promise);
  
  try {
    return await promise;
  } finally {
    inFlight.delete(key);
  }
}
```

## Deployment Strategies

### GitHub Actions CI/CD
```yaml
name: Deploy to Cloudflare

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy Worker
        uses: cloudflare/wrangler-action@v3
        with:
          apiToken: ${{ secrets.CF_API_TOKEN }}
          
      - name: Deploy Pages
        run: |
          npx wrangler pages deploy ./dist \
            --project-name=my-app
```

### Multi-Environment Setup
```bash
# Development
npx wrangler dev

# Staging deployment
npx wrangler deploy --env staging

# Production deployment
npx wrangler deploy --env production

# Gradual rollout
npx wrangler versions upload
npx wrangler versions deploy --percentage 10
```

## Troubleshooting Guide

### Common Issues

**Worker Not Deploying**
```bash
# Check configuration
npx wrangler whoami
npx wrangler config check

# Validate wrangler.toml
cat wrangler.toml | npx wrangler validate
```

**R2 Bucket Errors**
```bash
# Verify bucket exists
npx wrangler r2 bucket list

# Check CORS configuration
npx wrangler r2 bucket cors get my-bucket
```

**AI Model Errors**
```javascript
// Add error handling
try {
  const response = await env.AI.run(model, input);
  return Response.json(response);
} catch (error) {
  console.error('AI Error:', error);
  return Response.json({ 
    error: 'AI processing failed',
    fallback: true 
  }, { status: 500 });
}
```

## Best Practices

1. **Use Smart Placement**: Let Cloudflare optimize worker location
2. **Implement Caching**: Cache at edge for performance
3. **Handle Errors Gracefully**: Always have fallbacks
4. **Monitor Usage**: Use Workers Analytics
5. **Secure Secrets**: Use wrangler secrets, not env vars
6. **Test Locally**: Use `wrangler dev --local`

## Integration with Other Services

### With Cloudflare Pages
```toml
# Functions in pages
# functions/api/hello.js
export async function onRequest(context) {
  return new Response("Hello from Pages Functions!");
}
```

### With External APIs
```javascript
// Proxy external APIs
export default {
  async fetch(request, env) {
    const response = await fetch('https://api.external.com/data', {
      headers: {
        'Authorization': `Bearer ${env.EXTERNAL_API_KEY}`
      }
    });
    
    return new Response(response.body, {
      headers: {
        'Cache-Control': 'public, max-age=300',
        'Content-Type': 'application/json'
      }
    });
  }
};
```

## Tips for Success

1. **Start Small**: Deploy a simple Worker first
2. **Use Templates**: `npm create cloudflare@latest`
3. **Monitor Costs**: Check Workers & R2 usage
4. **Global by Default**: Design for edge deployment
5. **Stay Updated**: Check docs for new features

---

**Remember**: Cloudflare Workers run globally at the edge, providing unmatched performance. This agent helps you leverage the full power of Cloudflare's platform with up-to-date knowledge and best practices!