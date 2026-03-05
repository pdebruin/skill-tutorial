---
name: hybrid-foundry
description: >-
  Guide for working with Microsoft Foundry (Azure AI Foundry) — the unified enterprise AI platform
  for building, deploying, and governing AI agents and applications at scale. Use when the user
  mentions Microsoft Foundry, Azure AI Foundry, Foundry models, Foundry agents, Foundry IQ,
  Foundry SDK, AI agent deployment on Azure, model router, Azure AI project setup.
  Covers resource setup, model catalog and deployment, agent development, tool integration
  (MCP, A2A, function calling), evaluation, observability, and governance.
license: Apache-2.0
compatibility: >-
  Requires Azure CLI (az ≥2.67.0), and an active Azure subscription.
  Works with Python, JavaScript/TypeScript, C#, Java SDKs.
  Optionally uses Azure Developer CLI (azd) and Microsoft Learn MCP Server for live doc lookups.
metadata:
  author: m365 & ghcpcli
  version: "1.0"
allowed-tools: microsoft_docs_search microsoft_docs_fetch microsoft_code_sample_search
---

# Microsoft Foundry — Agent Skill

Help users work with Microsoft Foundry (formerly Azure AI Foundry), the enterprise AI platform for building, deploying, and governing AI agents and applications.

> **Golden rule:** Never invent CLI flags, SDK methods, or URLs. When in doubt, fetch the latest from Microsoft Learn via MCP before presenting commands to the user.

## When to Use This Skill

Activate when the user wants to:
- Set up Microsoft Foundry resources or projects
- Deploy or manage AI models (OpenAI, open-source, partner)
- Build, deploy, or manage AI agents (hosted or custom)
- Integrate agent tools (MCP, A2A, code interpreter, search, etc.)
- Evaluate, trace, or monitor AI agents and models
- Configure governance, security, or compliance controls
- Fine-tune models or configure model routing

Do **not** activate for unrelated Azure services or generic cloud questions.

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

**Key concepts:**
- A **Foundry Resource** (`AIServices` kind) is the top-level Azure resource managing governance, networking, and model deployments.
- A **Project** is a development boundary inside a resource where teams build agents, run evaluations, and manage files.
- Two portal experiences exist at [ai.azure.com](https://ai.azure.com): **new** (streamlined, Foundry-native) and **classic** (hub-based, multi-resource). Use **new** unless the user explicitly needs classic.
- SDK 2.x targets the **new** portal/API. SDK 1.x targets **classic**. They are incompatible.

---

## Decision Tree — What Do You Need?

```
What does the user want to do?

1. Create resources / set up a project
   → Use the Quick Setup below; for deeper options read references/RESOURCES-AND-SETUP.md

2. Deploy or choose a model / fine-tune a model
   → Read references/MODELS.md

3. Build an AI agent or integrate tools (MCP, search, code interpreter, etc.)
   → Read references/AGENTS.md

4. Evaluate, monitor, or trace agents
   → Read references/EVALUATION-AND-OBSERVABILITY.md

5. Governance, security, or compliance
   → Read references/GOVERNANCE.md

6. Understand the overall platform architecture
   → Read references/ARCHITECTURE.md
```

---

## Quick Setup (CLI-First — Handles 80% of Cases)

### Preconditions (check before provisioning)
1. **Region**: Confirm the target region supports the required models. If unsure, fetch via `microsoft_docs_search(query="Microsoft Foundry region support availability")`.
2. **RBAC**: User needs **Contributor** or **Owner** at subscription/RG scope to create resources; **Azure AI User** at project scope for day-to-day development.
3. **Portal track**: Confirm new vs classic — use **new** unless the user has hub-based projects.
4. **Enterprise network**: If behind a proxy or private endpoint, verify `az account show` works before proceeding.

### Step 1 — Authenticate

```bash
az login
az account set --subscription <SUBSCRIPTION_ID>
```

### Step 2 — Create resource group + Foundry resource + project

```bash
az group create --name my-foundry-rg --location eastus

az cognitiveservices account create \
    --name my-foundry-resource \
    --resource-group my-foundry-rg \
    --kind AIServices \
    --sku s0 \
    --location eastus \
    --allow-project-management

az cognitiveservices account project create \
    --name my-foundry-resource \
    --resource-group my-foundry-rg \
    --project-name my-foundry-project \
    --location eastus
```

### Step 3 — Deploy a model

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

### Step 4 — Install SDK + verify

| Language | Install |
|----------|---------|
| Python | `pip install --pre "azure-ai-projects>=2.0.0b4" azure-identity` |
| C# | `dotnet add package Azure.AI.Projects --prerelease` |
| TypeScript | `npm install @azure/ai-projects@beta @azure/identity` |
| Java | Maven beta dependency for `azure-ai-projects` |

Set environment variables and run a minimal model call:

```
PROJECT_ENDPOINT=https://<resource>.services.ai.azure.com/api/projects/<project>
MODEL_DEPLOYMENT_NAME=gpt-4.1-mini
```

All SDKs use `DefaultAzureCredential` (sign in with `az login` first). The SDK follows a two-client pattern: **AIProjectClient** for Foundry-native operations (connections, tracing) and an **OpenAI-compatible client** for model calls, agents, and evaluations.

> **Verify**: Fetch a quickstart snippet for the user's language via `microsoft_code_sample_search(query="Foundry quickstart <language> chat model")`, have them run it, and confirm a non-empty response.

---

## Troubleshooting

| Symptom | Resolution |
|---------|------------|
| `az login` / `az account show` fails | Re-authenticate; use device code if needed; confirm tenant/subscription |
| Permission denied creating resources | Need **Contributor**/**Owner** at subscription or RG scope |
| Permission denied creating agents | Need **Azure AI User** at project scope |
| Portal steps don't match UI | Confirm new vs classic portal; switch tracks |
| SDK import errors or missing methods | Verify correct SDK version (2.x preview vs 1.x GA); reinstall |
| Model/resource unavailable in region | Fetch current region guidance via MCP; choose available region |
| Agent doesn't call a tool | Confirm tool attached, model supports it; set `tool_choice` to `required` |
| CLI commands fail or flags unrecognized | Fetch latest docs via MCP; update Azure CLI to ≥2.67.0 |
| CLI/SDK calls time out | Check proxy settings and private endpoint policies |

---

## Common Pitfalls

1. **SDK version mismatch** — `azure-ai-projects` v2.x is NOT compatible with v1.x. Check imports.
2. **Resource vs. Project confusion** — A Foundry *resource* is the parent; *projects* live inside it.
3. **Model deployment naming** — Deployment names ≠ model names. You choose the deployment name.
4. **Region availability** — Not all models or features are available in all regions. Check the docs.
5. **azd vs. az** — `azd` manages full app lifecycle; `az` manages individual resources. Don't mix them up.
6. **Agent tools require configuration** — Tools must be explicitly enabled on the agent.
7. **MCP authentication** — MCP servers connected to Foundry may need separate auth configuration.

---

## Using MCP to Stay Current

Use Learn MCP tools to keep information current. Never invent CLI flags or URLs.

| Action | Tool call |
|--------|-----------|
| Find CLI commands | `microsoft_docs_search(query="Microsoft Foundry <action> CLI")` |
| Get full page content | `microsoft_docs_fetch(url="<page URL from search>")` |
| Find code samples | `microsoft_code_sample_search(query="Foundry <scenario>", language="<lang>")` |
| Check region/model availability | `microsoft_docs_search(query="Microsoft Foundry region support")` |

---

## Acceptance Criteria

A task is "done" when:
1. The appropriate portal track (new vs classic) and RBAC were chosen and justified.
2. The user's resource and project exist (or exact steps were provided) with **CLI commands sourced from Learn or this skill** — no invented flags, URLs, or tool names.
3. Steps are presented as a clear, ordered list with a **verification step** (minimal model call, agent test, or config check).
4. Risks and next steps were surfaced (regions, SDK versions, observability/tooling).
5. Common edge cases were handled: portal track, RBAC, region, SDK version, enterprise network.

---

## Reference Files (Load Only When Needed)

Detailed guidance is in the `references/` folder. Load only what the decision tree above indicates:

- [Architecture & Concepts](references/ARCHITECTURE.md) — Platform components, terminology, how things connect
- [Resource Setup](references/RESOURCES-AND-SETUP.md) — Creating resources, projects, dev environment (deep-dive beyond Quick Setup)
- [Models](references/MODELS.md) — Model catalog, deployment types, routing, fine-tuning
- [Agents](references/AGENTS.md) — Agent development, hosting, tools, publishing
- [Evaluation & Observability](references/EVALUATION-AND-OBSERVABILITY.md) — Evaluators, tracing, monitoring, red teaming
- [Governance](references/GOVERNANCE.md) — Control plane, security, compliance, cost management

---

## Key Resources & Live Documentation

| Resource | URL |
|----------|-----|
| Official docs | `https://learn.microsoft.com/en-us/azure/foundry/` |
| Foundry portal | `https://ai.azure.com` |
