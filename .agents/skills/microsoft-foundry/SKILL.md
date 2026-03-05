---
name: microsoft-foundry
description: >-
  Guide for working with Microsoft Foundry (Azure AI Foundry) — the unified enterprise AI platform
  for building, deploying, and governing AI agents and applications at scale. Use when the user
  mentions Microsoft Foundry, Azure AI Foundry, Foundry models, Foundry agents, Foundry IQ,
  Foundry SDK, AI agent deployment on Azure, model router, or Azure AI project setup.
  Covers resource setup, model catalog and deployment, agent development, tool integration
  (MCP, A2A, function calling), evaluation, observability, and governance.
license: Apache-2.0
compatibility: Requires Azure CLI (az), Azure Developer CLI (azd), and an active Azure subscription. Works with Python, JavaScript/TypeScript, C#, Java SDKs.
metadata:
  author: ghcpcli
  version: "1.0"
---

# Microsoft Foundry — Agent Skill

Help users work with Microsoft Foundry (formerly Azure AI Foundry), the enterprise AI platform for building, deploying, and governing AI agents and applications.

## When to Use This Skill

Activate when the user wants to:
- Set up Microsoft Foundry resources or projects
- Deploy or manage AI models (OpenAI, open-source, partner)
- Build, deploy, or manage AI agents (hosted or custom)
- Integrate agent tools (MCP, A2A, code interpreter, search, etc.)
- Evaluate, trace, or monitor AI agents and models
- Configure governance, security, or compliance controls
- Fine-tune models or configure model routing

---

## Quick Orientation

Microsoft Foundry is a **unified platform** that brings together:

| Pillar | What It Does |
|--------|-------------|
| **Foundry Models** | 1700+ models (OpenAI, Meta, Mistral, Cohere, etc.) with deployment and routing |
| **Agent Service** | Build, host, and publish AI agents with built-in tools |
| **Foundry IQ** | RAG engine — ground agents on enterprise data |
| **Control Plane** | Govern agents, models, and tools at scale |
| **AI Services** | Speech, vision, language, translation, content safety |
| **Evaluation** | Built-in evaluators, red teaming, CI/CD eval pipelines |

---

## Decision Tree — What Do You Need?

```
What does the user want to do?

1. Create resources / set up a project
   └── Read references/RESOURCES-AND-SETUP.md

2. Deploy or choose a model
   └── Read references/MODELS.md

3. Build or deploy an AI agent
   └── Read references/AGENTS.md

4. Integrate tools (MCP, search, code interpreter, etc.)
   └── Read references/AGENTS.md → Tools section

5. Evaluate, monitor, or trace agents
   └── Read references/EVALUATION-AND-OBSERVABILITY.md

6. Governance, security, or compliance
   └── Read references/GOVERNANCE.md

7. Understand the overall platform architecture
   └── Read references/ARCHITECTURE.md
```

---

## Development Options (Not Code — Just Options)

### SDK / Language Options

| Language | SDK Package | Notes |
|----------|------------|-------|
| **Python** | `azure-ai-projects` (v2.x) | Primary SDK, most samples available |
| **JavaScript/TypeScript** | `@azure/ai-projects` | Full parity with Python |
| **C#** | `Azure.AI.Projects` | .NET SDK, NuGet package |
| **Java** | `azure-ai-projects` | Maven package |
| **REST** | Direct HTTP | OpenAPI specs available |

### CLI Options

| Tool | Use Case |
|------|----------|
| `az` (Azure CLI) | Resource creation, model deployment, RBAC, configuration |
| `azd` (Azure Developer CLI) | End-to-end app lifecycle (init → provision → deploy) |
| `azd ai agent` extension | Agent-specific commands (init, deploy, manage) |
| **Portal** (ai.azure.com) | Visual UI for all operations, playgrounds, dashboards |
| **VS Code extension** | Integrated development with Foundry projects |

### Agent Framework Options

| Framework | Type | Best For |
|-----------|------|----------|
| **Foundry Agent Service** | Hosted (serverless) | Production agents with built-in tools |
| **Semantic Kernel** | Microsoft OSS | .NET/Python/Java orchestration |
| **AutoGen** | Microsoft OSS | Multi-agent conversations |
| **LangChain** | Community OSS | Python-first agent chains |
| **LlamaIndex** | Community OSS | RAG-focused agents |
| **CrewAI** | Community OSS | Role-based multi-agent |

### Authentication Options

| Method | When to Use |
|--------|------------|
| `DefaultAzureCredential` | SDK — auto-chains credentials (recommended) |
| `az login` | CLI — interactive developer auth |
| Service Principal | CI/CD and automation |
| Managed Identity | Production workloads on Azure |
| API Key | Simple scenarios (not recommended for production) |

---

## Key Resources & Live Documentation

| Resource | URL |
|----------|-----|
| Official docs | `https://learn.microsoft.com/en-us/azure/foundry/` |
| Foundry portal | `https://ai.azure.com` |
| Sample code | `https://github.com/microsoft-foundry/foundry-samples` |
| SDK overview | `https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/sdk-overview` |
| Model catalog | `https://ai.azure.com/explore/models` |
| MCP docs | `https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/model-context-protocol` |
| azd agent ext | `https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/extensions/azure-ai-foundry-extension` |

> **Tip:** For the latest information, use WebFetch on the URLs above. Documentation changes frequently as Foundry evolves.

---

## Common Pitfalls

1. **SDK version mismatch** — `azure-ai-projects` v2.x is NOT compatible with v1.x. Check imports.
2. **Resource vs. Project confusion** — A Foundry *resource* (AIServices kind) is the parent; *projects* live inside it.
3. **Model deployment naming** — Deployment names ≠ model names. You choose the deployment name; the model is the underlying engine.
4. **Region availability** — Not all models or features are available in all Azure regions. Check the docs.
5. **azd vs. az** — `azd` manages full app lifecycle (infra + code); `az` manages individual Azure resources. Don't mix them up.
6. **Agent tools require configuration** — Tools like code interpreter, file search, and web search must be explicitly enabled on the agent.
7. **MCP authentication** — MCP servers connected to Foundry may need separate auth configuration.

---

## Reference Files

Detailed guidance is in the `references/` folder. Load only what you need:

- [Architecture & Concepts](references/ARCHITECTURE.md) — Platform components, terminology, how things connect
- [Resource Setup](references/RESOURCES-AND-SETUP.md) — Creating resources, projects, dev environment
- [Models](references/MODELS.md) — Model catalog, deployment, routing, fine-tuning
- [Agents](references/AGENTS.md) — Agent development, hosting, tools, publishing
- [Evaluation & Observability](references/EVALUATION-AND-OBSERVABILITY.md) — Evaluators, tracing, monitoring, red teaming
- [Governance](references/GOVERNANCE.md) — Control plane, security, compliance, cost management
