# Microsoft Foundry — Governance

## Overview

The Foundry **Control Plane** provides centralized governance for agents, models, and tools at enterprise scale. It integrates with Azure's security, compliance, and cost management infrastructure.

## Control Plane Capabilities

| Capability | Description |
|------------|-------------|
| **Agent Management** | Register, manage, and monitor agents across the organization |
| **Model Governance** | Enforce token limits, apply guardrail policies, manage deployments |
| **Tool Governance** | Control which MCP tools and APIs agents can access |
| **Fleet Monitoring** | Cross-agent, cross-model health and performance dashboards |
| **Compliance & Security** | Enforce organizational policies, audit trail, data controls |

## Governing Agents

### Agent Registration
- Register custom (self-hosted) agents with the control plane
- Hosted agents are automatically registered
- Each agent gets metadata: name, description, owner, permissions

### Agent Lifecycle Management
| Action | How |
|--------|-----|
| **Register** | Portal or CLI — register agent with control plane |
| **Monitor** | Dashboard — view health, usage, errors |
| **Update** | Redeploy with new version |
| **Decommission** | Disable or remove agent registration |

### Fleet Management
- View all agents across the organization in one dashboard
- Compare performance metrics across agents
- Identify underperforming or unused agents

## Governing Models

### Token Limits
- Enforce per-deployment, per-project, or per-user token limits
- Prevent runaway costs from uncapped usage
- Configure via portal or CLI

### Guardrail Policies
- Apply content filtering rules to model outputs
- Built-in categories: violence, self-harm, sexual content, hate speech
- Custom blocklists for domain-specific terms
- Severity thresholds per category (low, medium, high)

### Model Deployment Policies
- Control which models can be deployed in which projects
- Restrict access to expensive or sensitive models
- Enforce deployment naming conventions

### Cost Optimization Options
| Strategy | Description |
|----------|-------------|
| **Model Router** | Automatically route to cost-optimal model per request |
| **PTU (Provisioned Throughput)** | Reserved capacity at predictable cost |
| **Batch Processing** | 50% cost reduction for non-latency-sensitive work |
| **Prompt Caching** | Reduce token costs for repeated prompts |
| **Token Limits** | Hard caps on token consumption |
| **Ask AI** | Built-in AI advisor for model/config optimization |

## Governing Tools

### MCP Tool Governance
- Route MCP tool calls through an **AI gateway**
- Apply rate limiting, authentication, and logging to tool access
- Block unauthorized tools
- Audit tool usage across agents

### Tool Catalog Management
- Maintain a curated catalog of approved tools
- Create private tool catalogs for organizational use
- Define tool best practices and usage guidelines

### AI Gateway Configuration
- Acts as a proxy between agents and external tools/APIs
- Provides: authentication, rate limiting, logging, content inspection
- Configure via portal or Bicep/ARM templates

## Security

### Identity & Access
| Feature | Description |
|---------|-------------|
| **Microsoft Entra ID** | SSO and identity management |
| **RBAC** | Role-based access at resource, project, and deployment level |
| **Managed Identity** | Passwordless auth for Azure resources |
| **Service Principals** | Automated/CI/CD authentication |
| **Conditional Access** | Location, device, risk-based access policies |

### Network Security
| Feature | Description |
|---------|-------------|
| **Private Endpoints** | Access Foundry via private network |
| **VNet Integration** | Deploy within virtual networks |
| **IP Firewall** | Restrict access by IP range |
| **Data Zone Deployments** | Keep data within specific geographic zones |

### Data Protection
- Data encrypted at rest and in transit
- Customer-managed keys (CMK) option
- Data residency controls via region and data zone selection
- Audit logs for all data access

## Compliance

### Standards
Microsoft Foundry inherits Azure's compliance certifications:
- SOC 1, SOC 2, SOC 3
- ISO 27001, ISO 27018
- HIPAA (with BAA)
- GDPR
- FedRAMP (select regions)
- And 90+ other certifications

### Audit & Logging
- All operations logged to Azure Activity Log
- Model inference logging to Azure Monitor
- Agent execution traces to Application Insights
- Exportable to SIEM systems (Microsoft Sentinel, Splunk, etc.)

### Responsible AI
- Content safety filters on all models
- Transparency notes for AI features
- Red teaming tools for vulnerability assessment
- Human-in-the-loop review capabilities

## Live Documentation

- Control plane overview: `https://learn.microsoft.com/en-us/azure/foundry/control-plane/overview`
- Manage agents at scale: `https://learn.microsoft.com/en-us/azure/foundry/control-plane/how-to-manage-agents`
- Token limits: `https://learn.microsoft.com/en-us/azure/foundry/control-plane/how-to-enforce-limits-models`
- Guardrail policies: `https://learn.microsoft.com/en-us/azure/foundry/control-plane/quickstart-create-guardrail-policy`
- Tool governance: `https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/governance`
- AI gateway: `https://learn.microsoft.com/en-us/azure/foundry/configuration/enable-ai-api-management-gateway-portal`
- Compliance: `https://learn.microsoft.com/en-us/azure/foundry/control-plane/how-to-manage-compliance-security`
