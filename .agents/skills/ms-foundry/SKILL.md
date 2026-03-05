---
name: ms-foundry
description: >
  Plan, create, configure, and operate Microsoft Foundry resources, projects, models, and agents.
  Use when asked about setting up Foundry (CLI or portal), choosing regions, assigning roles,
  installing SDKs, deploying models, creating agents, connecting tools, or troubleshooting Foundry
  environment issues. Covers provisioning, architecture, SDK usage, Agent Service, and best practices.
metadata:
  author: mcp skill creator
allowed-tools: microsoft_docs_search microsoft_docs_fetch microsoft_code_sample_search
---

# Microsoft Foundry

Microsoft Foundry is Azure's unified platform-as-a-service for enterprise AI operations, model hosting, agent development, and application building. It unifies agents, models, and tools under a single management grouping with built-in RBAC, networking, monitoring, and evaluations.

> **Note:** Azure AI Foundry was renamed to Microsoft Foundry. Documentation is being updated.

## 1 — When to Activate

Activate when the user wants to:

- Create a Foundry resource or project (CLI or portal)
- Deploy or call a model through Foundry
- Create or configure agents in Foundry Agent Service
- Set up SDKs or development environment for Foundry
- Choose regions, assign roles, or configure networking
- Troubleshoot Foundry auth, SDK, CLI, or agent issues
- Understand Foundry architecture (resources, projects, RBAC)

Do **not** activate for unrelated Azure services or generic cloud questions.

## 2 — Key Concepts

### Foundry Resource
Top-level Azure resource (`Microsoft.CognitiveServices/account`, kind `AIServices`) where you manage governance: networking, security, model deployments. The `--allow-project-management` flag enables project creation within the resource.

### Project
Development boundary inside a Foundry resource (`Microsoft.CognitiveServices/account/project`). Teams build agents, run evaluations, and manage files within project scope. RBAC can be scoped at both resource and project level.

### Foundry Portals
Two portal experiences at [ai.azure.com](https://ai.azure.com):

| Portal | When to use |
|--------|-------------|
| **Microsoft Foundry (new)** | Streamlined experience for building multi-agent apps. Only Foundry projects visible. |
| **Microsoft Foundry (classic)** | Working with multiple resource types: Azure OpenAI, hub-based projects, etc. |

Use **new** unless the user explicitly needs classic features.

### SDKs and Endpoints

| SDK | Purpose | Endpoint pattern |
|-----|---------|-----------------|
| **Foundry SDK** | Agents, evaluations, Foundry-specific features + OpenAI-compatible interfaces | `https://<resource>.services.ai.azure.com/api/projects/<project>` |
| **OpenAI SDK** | Full OpenAI API surface, Chat Completions | `https://<resource>.openai.azure.com/openai/v1` |
| **Foundry Tools SDKs** | Vision, Speech, Content Safety, etc. | Tool-specific endpoints |
| **Agent Framework** | Multi-agent orchestration in code (cloud-agnostic) | Uses project endpoint via Foundry SDK |

SDK packages (all **2.x preview** for the new Foundry API):

| Language | Package | Install |
|----------|---------|---------|
| Python | `azure-ai-projects` | `pip install --pre "azure-ai-projects>=2.0.0b4"` |
| C# | `Azure.AI.Projects` | `dotnet add package Azure.AI.Projects --prerelease` |
| TypeScript | `@azure/ai-projects` | `npm install @azure/ai-projects@beta @azure/identity` |
| Java | `azure-ai-projects` | Maven beta dependency |

> **Important:** SDK 2.x targets the **new** Foundry portal/API. SDK 1.x targets **classic**. They are incompatible.

### Agent Service
Create AI agents with custom instructions, augmented by tools (Code Interpreter, File Search, Bing Grounding, Azure AI Search, MCP, OpenAPI, Function calling, and more). Agents maintain conversation history across turns.

### RBAC

| Role | Scope | Purpose |
|------|-------|---------|
| **Contributor** or **Owner** | Subscription/RG | Create Foundry resources |
| **Azure AI Owner** | Subscription/RG | Create resources + manage access |
| **Azure AI User** | Resource or project | Day-to-day development (least privilege) |
| **Azure AI Developer** | Project | Create/edit agents, deploy models |

## 3 — Provisioning (CLI-First Workflow)

### Step 1 — Authenticate

```bash
az login
az account set --subscription <SUBSCRIPTION_ID>
```

### Step 2 — Create resource group

```bash
az group create --name my-foundry-rg --location eastus
```

### Step 3 — Create Foundry resource

```bash
az cognitiveservices account create \
    --name my-foundry-resource \
    --resource-group my-foundry-rg \
    --kind AIServices \
    --sku s0 \
    --location eastus \
    --allow-project-management
```

### Step 4 — Set custom subdomain (must be globally unique)

```bash
az cognitiveservices account update \
    --name my-foundry-resource \
    --resource-group my-foundry-rg \
    --custom-domain my-foundry-resource
```

### Step 5 — Create project

```bash
az cognitiveservices account project create \
    --name my-foundry-resource \
    --resource-group my-foundry-rg \
    --project-name my-foundry-project \
    --location eastus
```

### Step 6 — Deploy a model

```bash
az cognitiveservices account deployment create \
    --name my-foundry-resource \
    --resource-group my-foundry-rg \
    --deployment-name gpt-4.1-mini \
    --model-name gpt-4.1-mini \
    --model-version "2025-04-14" \
    --model-format OpenAI \
    --sku-capacity 10 \
    --sku-name Standard
```

### Step 7 — Grant access to team members

```bash
PROJECT_ID=$(az cognitiveservices account project show \
  --name my-foundry-resource \
  --resource-group my-foundry-rg \
  --project-name my-foundry-project \
  --query id -o tsv)

az role assignment create \
    --role "Azure AI User" \
    --assignee "user@contoso.com" \
    --scope $PROJECT_ID
```

> **Always fetch the latest CLI commands from Learn via MCP before presenting them.** CLI flags and commands may change.

## 4 — Using the SDK

All languages follow the same two-client pattern:

1. **AIProjectClient** — Foundry-native operations (connections, project properties, tracing)
2. **OpenAI-compatible client** — Model calls (Responses API), agents, evaluations

### Environment Variables

```
PROJECT_ENDPOINT=https://<resource>.services.ai.azure.com/api/projects/<project>
MODEL_DEPLOYMENT_NAME=gpt-4.1-mini
```

### Authentication

All SDKs use `DefaultAzureCredential` (Microsoft Entra ID). Sign in with `az login` before running code. API keys also work on the `/openai/v1` endpoint.

### Common Operations

To find working code for any SDK operation, use:
- `microsoft_code_sample_search(query="Foundry quickstart <language> <operation>")`
- `microsoft_docs_fetch(url="https://learn.microsoft.com/azure/foundry/quickstarts/get-started-code")`

## 5 — Agent Service

### Creating Agents

Agents define core behavior (instructions, model, tools). Once created, they ensure consistent responses without repeating instructions.

### Adding Tools

| Tool | Purpose |
|------|---------|
| Code Interpreter | Execute code, generate files |
| File Search | Search uploaded files |
| Function calling | Call custom functions |
| Azure AI Search | Search indexed data |
| Bing Search / Custom | Web grounding |
| MCP | Connect to MCP servers |
| OpenAPI | Call REST APIs |
| SharePoint | Enterprise content |
| Browser Automation | Web interactions |
| Agent2Agent (A2A) | Inter-agent communication |

For tool support by model and region:
- `microsoft_docs_fetch(url="https://learn.microsoft.com/azure/foundry/agents/concepts/tool-best-practice")`

### Tool Best Practices

- Describe what each tool does in agent instructions
- Use `tool_choice` for deterministic control (`auto`, `required`, `none`)
- Treat tool outputs as untrusted input; validate before acting
- Never include credentials in prompts or tool calls
- Add decision rules when tools overlap

## 6 — Architecture Decisions

### Region and Quotas
Confirm model availability in your target region before provisioning. Fetch current availability:
- `microsoft_docs_search(query="Microsoft Foundry region support availability")`

### Networking
- Default: public access
- Private link available for network isolation
- Container injection for agent-to-resource communication within VNets
- End-to-end isolation: use classic experience, SDK, or CLI (not yet fully supported in new portal)

### Data Storage
- **Default**: Microsoft-managed storage (logically separated)
- **Bring your own storage**: Optional for files and agent state (threads, messages)
- **Customer-managed keys**: Supported with Azure Key Vault (same region, soft delete + purge protection enabled)

### Monitoring
Azure Monitor metrics segmented by scope (resource-level and project-level). Agent activity, evaluation performance tracked per project.

## 7 — Troubleshooting

| Issue | Resolution |
|-------|------------|
| `az login` or `az account show` fails | Re-authenticate; use device code if needed; confirm tenant/subscription |
| Permission denied creating resources | Need **Contributor**/**Owner** at subscription or RG scope |
| Permission denied creating agents | Need **Azure AI User** at project scope |
| Portal steps don't match UI | Confirm new vs classic portal; switch tracks |
| SDK import errors or missing methods | Verify correct SDK version (2.x preview vs 1.x GA); reinstall |
| Model/resource unavailable in region | Fetch current region guidance; choose available region |
| Agent doesn't call a tool | Confirm tool attached, model supports it; set `tool_choice` to `required` |
| Tool calls return empty results | Improve tool descriptions; verify data is indexed/searchable |
| Tool calls fail | Verify config, auth, and endpoint reachability |
| CLI commands fail or flags unrecognized | Fetch latest docs via MCP; update Azure CLI to ≥2.67.0 |

For deeper troubleshooting:
- `microsoft_docs_search(query="Microsoft Foundry troubleshoot <symptom>")`

## 8 — Learn MCP Usage

Use Learn MCP tools to keep information current. Never invent CLI flags or URLs.

| Action | Tool call |
|--------|-----------|
| Find CLI commands | `microsoft_docs_search(query="Microsoft Foundry <action> CLI")` |
| Get full page content | `microsoft_docs_fetch(url="<page URL from search>")` |
| Find code samples | `microsoft_code_sample_search(query="Foundry <scenario>", language="<lang>")` |
| Check region/model availability | `microsoft_docs_search(query="Microsoft Foundry region support")` |
| SDK installation steps | `microsoft_docs_fetch(url="https://learn.microsoft.com/azure/foundry/how-to/develop/sdk-overview")` |
| Agent tool support matrix | `microsoft_docs_fetch(url="https://learn.microsoft.com/azure/foundry/agents/concepts/tool-best-practice")` |

## 9 — Key References (Resolve via MCP)

| Topic | URL |
|-------|-----|
| What is Microsoft Foundry? | https://learn.microsoft.com/azure/foundry/what-is-foundry |
| Architecture | https://learn.microsoft.com/azure/foundry/concepts/architecture |
| Set up resources (quickstart) | https://learn.microsoft.com/azure/foundry/tutorials/quickstart-create-foundry-resources |
| Get started with code | https://learn.microsoft.com/azure/foundry/quickstarts/get-started-code |
| SDK overview | https://learn.microsoft.com/azure/foundry/how-to/develop/sdk-overview |
| Prepare dev environment | https://learn.microsoft.com/azure/foundry/how-to/develop/install-cli-sdk |
| Agent Service tools | https://learn.microsoft.com/azure/foundry/agents/concepts/tool-best-practice |
| RBAC for Foundry | https://learn.microsoft.com/azure/foundry/concepts/rbac-foundry |
| Region support | https://learn.microsoft.com/azure/foundry/reference/region-support |
| Foundry docs hub | https://learn.microsoft.com/azure/foundry/ |
