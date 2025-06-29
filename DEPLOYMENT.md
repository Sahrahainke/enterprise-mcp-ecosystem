🚀 Deployment Guide - Enterprise MCP Ecosystem
📋 Prerequisites Checklist
Before deploying the Enterprise MCP Ecosystem, ensure you have:
🛠️ Development Environment
 Node.js 18+ installed
 TypeScript 4.8+ globally installed
 Git configured with SSH keys
 Claude Desktop application installed
 Code Editor (VS Code recommended)
🔑 API Access & Credentials
 HubSpot API Key (Marketing Hub Professional+)
 Microsoft Teams webhook URL and app registration
 GitHub Personal Access Token (if using GitHub integration)
 Weather API Key (OpenWeatherMap or similar)
💼 Business Accounts
 HubSpot Marketing Hub subscription
 Microsoft Teams business account
 GitHub account for repository hosting
🎯 Quick Start (15 Minutes)
Step 1: Repository Setup (3 min)
bash
# Clone the repository
git clone https://github.com/[your-username]/enterprise-mcp-ecosystem.git
cd enterprise-mcp-ecosystem

# Install dependencies
npm install
npm run install-all
Step 2: Environment Configuration (5 min)
bash
# Copy environment template
cp .env.template .env

# Edit with your API keys
nano .env
Required Environment Variables:
bash
# HubSpot Configuration
HUBSPOT_API_KEY=your_hubspot_private_app_token
HUBSPOT_PORTAL_ID=your_portal_id

# Microsoft Teams Configuration  
TEAMS_WEBHOOK_URL=https://outlook.office.com/webhook/your-webhook-url
TEAMS_APP_ID=your_teams_app_id
TEAMS_APP_PASSWORD=your_teams_app_password

# GitHub Integration (Optional)
GITHUB_TOKEN=ghp_your_personal_access_token

# Weather API (Optional)
WEATHER_API_KEY=your_openweathermap_api_key

# File System Paths
MARKETING_DOCS_PATH=/path/to/your/marketing/documents
PROJECT_FILES_PATH=/path/to/your/project/files
Step 3: Build All Servers (2 min)
bash
# Build TypeScript to JavaScript
npm run build-all

# Verify builds completed successfully
npm run health-check
Step 4: Claude Desktop Configuration (3 min)
Edit your Claude Desktop configuration file:
macOS: ~/Library/Application Support/Claude/claude_desktop_config.json Windows: %APPDATA%\Claude\claude_desktop_config.json
json
{
  "mcpServers": {
    "hello-world": {
      "command": "node",
      "args": ["/path/to/enterprise-mcp-ecosystem/servers/hello-world/build/index.js"]
    },
    "calculator": {
      "command": "node", 
      "args": ["/path/to/enterprise-mcp-ecosystem/servers/calculator/build/index.js"]
    },
    "filesystem": {
      "command": "node",
      "args": ["/path/to/enterprise-mcp-ecosystem/servers/filesystem/build/index.js"],
      "env": {
        "MARKETING_DOCS_PATH": "/path/to/marketing/documents"
      }
    },
    "api-integration": {
      "command": "node",
      "args": ["/path/to/enterprise-mcp-ecosystem/servers/api-integration/build/index.js"],
      "env": {
        "GITHUB_TOKEN": "your_github_token",
        "WEATHER_API_KEY": "your_weather_api_key"
      }
    },
    "task-management": {
      "command": "node",
      "args": ["/path/to/enterprise-mcp-ecosystem/servers/task-management/build/index.js"]
    },
    "hubspot": {
      "command": "node",
      "args": ["/path/to/enterprise-mcp-ecosystem/servers/hubspot/build/index.js"],
      "env": {
        "HUBSPOT_API_KEY": "your_hubspot_api_key"
      }
    },
    "teams": {
      "command": "node",
      "args": ["/path/to/enterprise-mcp-ecosystem/servers/teams/build/index.js"],
      "env": {
        "TEAMS_WEBHOOK_URL": "your_teams_webhook_url"
      }
    }
  }
}
Step 5: Test & Verify (2 min)
bash
# Restart Claude Desktop
# Test with: "List all available MCP tools"
# Expected: Should see tools from all 7 servers
🔧 Detailed Configuration
🎯 HubSpot Integration Setup
1. Create Private App in HubSpot
Go to Settings → Integrations → Private Apps
Click Create a private app
Configure scopes:
✅ crm.objects.contacts.read
✅ crm.objects.contacts.write
✅ crm.objects.companies.read
✅ crm.objects.companies.write
✅ content.read
✅ content.write
✅ automation.read
Copy the access token
2. Test HubSpot Connection
bash
# Test API connection
curl -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     "https://api.hubapi.com/crm/v3/objects/contacts?limit=1"
👥 Microsoft Teams Integration Setup
1. Create Teams App Registration
Go to Azure Portal → App registrations
Click New registration
Configure:
Name: Enterprise MCP Integration
Supported account types: Single tenant
Redirect URI: Not needed for webhook
Note the Application (client) ID
2. Create Incoming Webhook
In Teams, go to your channel
Click ⋯ → Connectors → Incoming Webhook
Configure webhook and copy URL
3. Test Teams Integration
bash
# Test webhook
curl -X POST YOUR_WEBHOOK_URL \
     -H "Content-Type: application/json" \
     -d '{"text": "Test message from MCP Ecosystem"}'
📁 File System Setup
1. Organize Marketing Documents
bash
# Recommended folder structure
marketing-documents/
├── campaigns/
│   ├── 2024-q1/
│   ├── 2024-q2/
│   └── templates/
├── assets/
│   ├── images/
│   ├── videos/
│   └── documents/
├── analytics/
│   ├── reports/
│   └── dashboards/
└── team/
    ├── guidelines/
    ├── processes/
    └── training/
2. Set Permissions
bash
# Ensure Node.js has read access
chmod -R 755 /path/to/marketing-documents
🎭 Environment-Specific Configurations
🏗️ Development Environment
claude_desktop_config.json (Development):
json
{
  "mcpServers": {
    "hello-world": {
      "command": "npm",
      "args": ["run", "dev"],
      "cwd": "/path/to/servers/hello-world",
      "env": {
        "NODE_ENV": "development",
        "LOG_LEVEL": "debug"
      }
    }
    // ... other servers with dev configuration
  }
}
🎪 Staging Environment
Configuration for testing:
json
{
  "mcpServers": {
    "hubspot": {
      "command": "node",
      "args": ["build/index.js"],
      "cwd": "/path/to/servers/hubspot",
      "env": {
        "NODE_ENV": "staging",
        "HUBSPOT_API_KEY": "test_api_key",
        "RATE_LIMIT_ENABLED": "true"
      }
    }
    // ... staging configurations
  }
}
🏭 Production Environment
Production-ready configuration:
json
{
  "mcpServers": {
    "hubspot": {
      "command": "node",
      "args": ["build/index.js"],
      "cwd": "/path/to/servers/hubspot",
      "env": {
        "NODE_ENV": "production",
        "HUBSPOT_API_KEY": "prod_api_key",
        "CACHE_ENABLED": "true",
        "METRICS_ENABLED": "true",
        "LOG_LEVEL": "info"
      }
    }
    // ... production configurations
  }
}
🐳 Docker Deployment
📦 Containerized Setup
Dockerfile for production:
dockerfile
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
COPY servers/*/package*.json ./servers/*/
RUN npm run install-all

COPY . .
RUN npm run build-all

FROM node:18-alpine AS runtime
WORKDIR /app

# Copy built applications
COPY --from=builder /app/servers/*/build ./servers/*/build
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD npm run health-check

EXPOSE 3000
CMD ["npm", "start"]
Docker Compose for local development:
yaml
version: '3.8'
services:
  mcp-ecosystem:
    build: .
    environment:
      - NODE_ENV=development
      - HUBSPOT_API_KEY=${HUBSPOT_API_KEY}
      - TEAMS_WEBHOOK_URL=${TEAMS_WEBHOOK_URL}
    volumes:
      - ./marketing-documents:/app/data/marketing-documents:ro
      - ./logs:/app/logs
    ports:
      - "3000:3000"
    restart: unless-stopped
    
  # Optional: Add monitoring
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml
☸️ Kubernetes Deployment
🚀 Production Kubernetes Setup
Namespace and ConfigMap:
yaml
apiVersion: v1
kind: Namespace
metadata:
  name: mcp-ecosystem

---
apiVersion: v1
kind: ConfigMap
metadata:
  name: mcp-config
  namespace: mcp-ecosystem
data:
  NODE_ENV: "production"
  LOG_LEVEL: "info"
  CACHE_ENABLED: "true"
Secrets:
yaml
apiVersion: v1
kind: Secret
metadata:
  name: mcp-secrets
  namespace: mcp-ecosystem
type: Opaque
stringData:
  HUBSPOT_API_KEY: "your_hubspot_key"
  TEAMS_WEBHOOK_URL: "your_teams_webhook"
  GITHUB_TOKEN: "your_github_token"
Deployment:
yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mcp-ecosystem
  namespace: mcp-ecosystem
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mcp-ecosystem
  template:
    metadata:
      labels:
        app: mcp-ecosystem
    spec:
      containers:
      - name: mcp-servers
        image: enterprise-mcp:latest
        envFrom:
        - configMapRef:
            name: mcp-config
        - secretRef:
            name: mcp-secrets
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5
📊 Monitoring & Observability
📈 Performance Monitoring Setup
Prometheus Configuration:
yaml
# prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'mcp-ecosystem'
    static_configs:
      - targets: ['mcp-ecosystem:3000']
    metrics_path: '/metrics'
    scrape_interval: 5s
Grafana Dashboard:
json
{
  "dashboard": {
    "id": null,
    "title": "MCP Ecosystem Metrics",
    "panels": [
      {
        "title": "Request Rate",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(mcp_requests_total[5m])",
            "legendFormat": "{{server}}"
          }
        ]
      },
      {
        "title": "Response Time",
        "type": "graph", 
        "targets": [
          {
            "expr": "histogram_quantile(0.95, mcp_response_duration_seconds_bucket)",
            "legendFormat": "95th percentile"
          }
        ]
      }
    ]
  }
}
🔍 Troubleshooting Guide
❌ Common Issues & Solutions
Issue: "Server not found" in Claude
Symptoms: Claude doesn't see MCP servers Solutions:
Check Claude Desktop configuration file path
Verify JSON syntax in config file
Ensure server build completed successfully
Restart Claude Desktop application
bash
# Debug steps
npm run build-all
npm run health-check
# Check Claude Desktop logs (macOS)
tail -f ~/Library/Logs/Claude/claude-desktop.log
Issue: HubSpot API Authentication Failed
Symptoms: "401 Unauthorized" errors Solutions:
Verify API key is correct
Check HubSpot app scopes
Ensure API key environment variable is set
bash
# Test API key
curl -H "Authorization: Bearer $HUBSPOT_API_KEY" \
     "https://api.hubapi.com/crm/v3/objects/contacts?limit=1"
Issue: Teams Integration Not Working
Symptoms: Messages not appearing in Teams Solutions:
Verify webhook URL is correct
Check Teams channel permissions
Test webhook manually
bash
# Test webhook
curl -X POST "$TEAMS_WEBHOOK_URL" \
     -H "Content-Type: application/json" \
     -d '{"text": "Test message"}'
🔧 Debug Mode
Enable detailed logging:
bash
# Set debug environment
export DEBUG=mcp:*
export LOG_LEVEL=debug

# Run specific server in debug mode
cd servers/hubspot
npm run dev -- --verbose
📝 Log Analysis
Log file locations:
Development: logs/development.log
Production: logs/production.log
Errors: logs/errors.log
Common log patterns:
bash
# Find authentication errors
grep "401\|403\|Unauthorized" logs/*.log

# Monitor API rate limits
grep "rate.limit\|429" logs/*.log

# Check server startup issues
grep "server.start\|initialization" logs/*.log
🎯 Performance Optimization
⚡ Performance Tuning
1. Caching Configuration
typescript
// Enable caching for external APIs
const cacheConfig = {
  hubspot: { ttl: 300 }, // 5 minutes
  weather: { ttl: 600 }, // 10 minutes
  github: { ttl: 900 }   // 15 minutes
};
2. Connection Pooling
typescript
// API connection pooling
const poolConfig = {
  maxConnections: 10,
  timeout: 5000,
  retries: 3
};
3. Resource Limits
bash
# Set Node.js memory limits
export NODE_OPTIONS="--max-old-space-size=512"
🔄 Maintenance & Updates
📅 Regular Maintenance Tasks
Weekly Tasks
 Review server logs for errors
 Check API rate limit usage
 Update dependencies if needed
 Backup configuration files
Monthly Tasks
 Review performance metrics
 Update API keys if expiring
 Security audit of configurations
 Documentation updates
Update Process
bash
# Backup current configuration
cp claude_desktop_config.json claude_desktop_config.json.backup

# Pull latest changes
git pull origin main

# Update dependencies
npm update
npm run install-all

# Rebuild servers
npm run build-all

# Test before deploying
npm run test-all
🆘 Support & Resources
📞 Getting Help
Documentation:
GitHub Repository
Wiki Pages
API Reference
Community Support:
GitHub Discussions
GitHub Issues
Professional Support:
Email: support@enterprise-mcp.dev
Business inquiries: business@enterprise-mcp.dev
✅ Post-Deployment Checklist
After successful deployment:
 All 7 servers respond to health checks
 Claude can list tools from all servers
 HubSpot integration creates test contact
 Teams integration sends test message
 File system server can search documents
 Task management creates and lists tasks
 API integrations return current data
 Monitoring systems are collecting metrics
 Backup procedures are in place
 Team has access to documentation
🎉 Congratulations! Your Enterprise MCP Ecosystem is now live!
