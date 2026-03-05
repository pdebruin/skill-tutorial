# Microsoft Foundry — Resource Setup (Deep Dive)

> **Note:** For the standard CLI-first setup path, see the Quick Setup section in SKILL.md. This file covers alternative setup options and additional configuration.

## Alternative Setup Options

### Azure Portal (UI)
1. Go to `https://ai.azure.com`
2. Click "Create a project"
3. Follow the wizard — it creates the resource and project together
4. Choose region, pricing tier, and networking options

### Azure Developer CLI (`azd`)
```
# Initialize from a template
azd init -t Azure-Samples/azd-ai-starter-basic --location <region>

# Provision all infrastructure (Bicep-based)
azd provision

# Full deploy (infra + code)
azd up
```

### SDKs (Programmatic)
- **Python**: Use `azure-mgmt-cognitiveservices` for resource management
- **Bicep/ARM**: Infrastructure-as-code templates for repeatable deployments
- **Terraform**: AzureRM provider supports cognitive services resources

### VS Code Extension
- Install the Azure AI Foundry extension for VS Code
- Create and manage projects directly from the IDE

## Developer Environment Prerequisites

| Tool | Minimum Version | Purpose |
|------|----------------|---------|
| Azure CLI (`az`) | 2.67.0+ | Resource management |
| Azure Developer CLI (`azd`) | 1.21.3+ | App lifecycle management |
| Python | 3.8+ | Python SDK development |
| Node.js | 18+ | JavaScript/TypeScript SDK development |
| .NET | 8.0+ | C# SDK development |
| Docker | Latest | Hosted agent containers |

> **Important:** SDK 2.x (preview/beta) targets the **new** Foundry portal/API. SDK 1.x targets **classic**. They are incompatible. SDK install commands and auth guidance are in SKILL.md Quick Setup.

## Project Configuration

### Connections
Projects can have **connections** to external services:
- Azure AI Search (for RAG)
- Azure Blob Storage (for data)
- Azure Key Vault (for secrets)
- Custom APIs (via connection strings)

## Live Documentation

- Quickstart: `https://learn.microsoft.com/en-us/azure/foundry/tutorials/quickstart-create-foundry-resources`
- Dev environment setup: `https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/install-cli-sdk`
- VS Code integration: `https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/get-started-projects-vs-code`
