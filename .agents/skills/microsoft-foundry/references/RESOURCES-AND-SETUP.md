# Microsoft Foundry — Resource Setup

## Overview

Before using Microsoft Foundry, you need to create a **Foundry resource** and at least one **project**. There are multiple ways to do this.

## Setup Options

### Option 1: Azure Portal (UI)
1. Go to `https://ai.azure.com`
2. Click "Create a project"
3. Follow the wizard — it creates the resource and project together
4. Choose region, pricing tier, and networking options

### Option 2: Azure CLI (`az`)
```
# Create resource group
az group create --name <rg-name> --location <region>

# Create Foundry resource
az cognitiveservices account create \
  --name <resource-name> \
  --resource-group <rg-name> \
  --kind AIServices \
  --sku s0 \
  --location <region> \
  --allow-project-management

# Create a project within the resource
az cognitiveservices account project create \
  --name <resource-name> \
  --resource-group <rg-name> \
  --project-name <project-name> \
  --location <region>
```

### Option 3: Azure Developer CLI (`azd`)
```
# Initialize from a template
azd init -t Azure-Samples/azd-ai-starter-basic --location <region>

# Provision all infrastructure (Bicep-based)
azd provision

# Full deploy (infra + code)
azd up
```

### Option 4: SDKs (Programmatic)
- **Python**: Use `azure-mgmt-cognitiveservices` for resource management
- **Bicep/ARM**: Infrastructure-as-code templates for repeatable deployments
- **Terraform**: AzureRM provider supports cognitive services resources

### Option 5: VS Code Extension
- Install the Azure AI Foundry extension for VS Code
- Create and manage projects directly from the IDE

## Developer Environment Setup

### Prerequisites
| Tool | Minimum Version | Purpose |
|------|----------------|---------|
| Azure CLI (`az`) | 2.67.0+ | Resource management |
| Azure Developer CLI (`azd`) | 1.21.3+ | App lifecycle management |
| Python | 3.8+ | Python SDK development |
| Node.js | 18+ | JavaScript/TypeScript SDK development |
| .NET | 8.0+ | C# SDK development |
| Docker | Latest | Hosted agent containers |

### SDK Installation

**Python:**
```
pip install azure-ai-projects azure-identity
```

**JavaScript/TypeScript:**
```
npm install @azure/ai-projects @azure/identity
```

**C#:**
```
dotnet add package Azure.AI.Projects
```

**Java:**
```
<!-- Maven dependency -->
<dependency>
  <groupId>com.azure</groupId>
  <artifactId>azure-ai-projects</artifactId>
</dependency>
```

### Authentication Setup

1. **For development**: `az login` (interactive browser auth)
2. **For CI/CD**: Create a service principal with `az ad sp create-for-rbac`
3. **For production**: Use Managed Identity on Azure resources
4. **In code**: Always use `DefaultAzureCredential` — it auto-chains the right method

### Environment Variables

Set these for SDK usage:
```
PROJECT_ENDPOINT=https://<your-resource>.services.ai.azure.com/api/projects/<project-name>
MODEL_DEPLOYMENT_NAME=<your-deployment-name>
```

## Project Configuration

### Connections
Projects can have **connections** to external services:
- Azure AI Search (for RAG)
- Azure Blob Storage (for data)
- Azure Key Vault (for secrets)
- Custom APIs (via connection strings)

### Access Control (RBAC)
Key roles:
| Role | Can Do |
|------|--------|
| **Azure AI Developer** | Full access to project resources, deploy models, create agents |
| **Azure AI Inference Deployment Operator** | Deploy and manage model deployments |
| **Contributor** | Manage Azure resources but not data plane |
| **Reader** | View-only access |

## Live Documentation

- Quickstart: `https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources`
- Dev environment setup: `https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/install-cli-sdk`
- VS Code integration: `https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/get-started-projects-vs-code`
