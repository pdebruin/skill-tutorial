---
name: m365-foundry
description: >
  Plan, create, configure, and operate Microsoft Foundry resources and projects using Azure CLI,
  SDKs, and Learn MCP Server. Use when asked about setting up Foundry, choosing regions and roles,
  installing SDKs, creating agents, calling models, or troubleshooting environment issues.
license: Apache-2.0
compatibility: >
  Works with agents that support Agent Skills and can access the Microsoft Learn MCP Server.
  Requires Azure CLI and appropriate RBAC (Contributor or Owner for creation; Azure AI User for using projects).
metadata:
  author: m365
  version: "2.0"
allowed-tools: microsoft_docs_search microsoft_docs_fetch microsoft_code_sample_search
---

# Microsoft Foundry – Operator & Setup Skill (Version 2)

This skill helps the agent make sound decisions, follow safe procedures, fetch up-to-date commands from Microsoft Learn via MCP, and avoid inventing CLI syntax. It covers: region selection, RBAC, resource creation, project creation, SDK setup, model calls, and troubleshooting.

## 1. When to activate this skill
Activate when the user wants to:
- create a Microsoft Foundry resource or project (CLI or portal)
- choose a region, role, or SDK path
- set up or fix the Foundry development environment
- run a model through Foundry, create an agent, or verify configuration
- understand the new vs classic portals and which to use

Do not activate for unrelated Azure services or generic cloud questions.

## 2. Orientation (load only if needed)
Microsoft Foundry is Azure’s unified platform for AI apps, agents, and models, documented with both “new” and “classic” portal tracks. Use the track that matches the user’s environment, as shown by the toggle in docs. Use the Microsoft Learn MCP Server to fetch exact quickstart steps and CLI blocks instead of remembering them.

## 3. Preconditions and decisions (must check first)
1) Region availability: confirm an allowed region for the user and the required models; if unsure, fetch current guidance from Foundry docs via MCP.  
2) RBAC: verify the user has sufficient rights (Contributor/Owner to create resources; Azure AI User to build on an existing project).  
3) Portal mode: confirm whether the user is on the new or classic portal; follow steps for that track.  
4) Enterprise network: if behind a proxy or private endpoint, verify Azure CLI connectivity (e.g., `az account show`) before provisioning; configure proxy if needed.

## 4. Minimal viable setup (CLI-first workflow)
Use this sequence when the user wants a reliable path to a working Foundry project.

Step 1 — Authenticate
- Ask user to run `az login` and `az account set --subscription <SUB_ID>`; confirm tenant and subscription.

Step 2 — Create a Foundry resource
- Never invent CLI flags. Always fetch the latest commands with MCP:
  - microsoft_docs_search("Microsoft Foundry quickstart create resource CLI")
  - microsoft_docs_fetch("<chosen_page_url>")
- Present the CLI block exactly as in the docs.

Step 3 — Create a Foundry project
- Use CLI or portal steps as documented. Fetch the correct sequence (e.g., flags like `--allow-project-management`) via MCP and present it verbatim.

Step 4 — Prepare the development environment
- Follow the docs to install language runtimes, Azure CLI, extensions, and the correct preview SDK packages where required. Fetch the current instructions via MCP.

Step 5 — Verify with a minimal model call
- Fetch a current language snippet (Python/.NET/TypeScript/Java) via:
  - microsoft_code_sample_search("Foundry quickstart Python chat model")
- Instruct the user to set `PROJECT_ENDPOINT` and `MODEL_DEPLOYMENT_NAME`, then run the sample.

Step 6 — Next steps
- Offer appropriate follow-ups: deploy an agent, add tools (including MCP integrations), or enable observability via the control plane.

## 5. Standard operating procedure (SOP)
1) Clarify intent: model call vs full agent workflow; new vs classic; CLI vs portal.  
2) Validate region and RBAC.  
3) Provision: resource → project using commands fetched via MCP.  
4) Bootstrap SDK: install per docs; run the minimal model-call sample.  
5) Summarize results and propose a next action.

## 6. Output expectations
The agent must produce:
- exact CLI commands copied from Microsoft Learn (no invented flags)
- a clear, ordered list of steps
- a minimal model-call confirmation
- a brief summary of achievements and the next action
- no hallucinated URLs, flags, or tool names

## 7. Troubleshooting
Authentication
- If `az login` or `az account show` fails, re-authenticate and confirm tenant/subscription; use device code if needed.

Portal version mismatch
- If steps don’t match the UI, confirm whether the user is in the new or classic portal and switch tracks accordingly.

RBAC denied
- If creation fails with authorization errors, verify required roles and retry with sufficient permissions or a different scope.

SDK mismatch
- If code errors cite missing methods, fetch the SDK quickstart again and install the documented preview or prerelease version.

Region or quota issues
- If a model or resource is unavailable, fetch current region availability guidance and pick an available region or SKU.

Enterprise networks
- If CLI/SDK calls time out, confirm proxy settings and private endpoint policies; test with a simple CLI call before provisioning.

## 8. MCP usage rules for this skill
Use Learn MCP to stay current for: region availability, CLI resource creation, project creation, SDK installation, language samples, and troubleshooting guidance. Do not embed long documentation excerpts; fetch what you need when you need it.

## 9. Acceptance criteria (CAA-style)
Success criteria:
- The skill activates only for Foundry setup and operations tasks.
- Commands are fetched from Learn via MCP before presentation.
- The workflow follows the order: region/RBAC → resource → project → SDK → model call.
- No hallucinated commands/URLs; minimal, verifiable outputs are provided.
- Common edge cases are handled: portal track, RBAC, region, SDK version, enterprise network.

## 10. References (resolve at runtime via MCP)
- Microsoft Foundry docs hub: https://learn.microsoft.com/en-us/azure/foundry/  
- What is Microsoft Foundry (new vs classic): https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry  
- Quickstart: get started with code: https://learn.microsoft.com/en-us/azure/foundry/quickstarts/get-started-code  
- Prepare your development environment: https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/install-cli-sdk  
- Create resource (Azure CLI/portal): https://learn.microsoft.com/en-us/azure/ai-services/multi-service-resource  
- Create a Foundry project (CLI/portal): https://learn.microsoft.com/en-us/azure/foundry/how-to/create-projects  
- Learn MCP Server overview: https://learn.microsoft.com/en-us/training/support/mcp
