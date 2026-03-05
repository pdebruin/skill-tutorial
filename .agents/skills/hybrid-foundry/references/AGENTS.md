# Microsoft Foundry — Agent Development

## Overview

Microsoft Foundry provides a complete platform for building, deploying, and managing AI agents. Agents can be **hosted** (serverless on Foundry) or **custom** (self-hosted, registered with Foundry).

## Agent Types

| Type | Description | Hosting |
|------|-------------|---------|
| **Hosted Agent** | Serverless, fully managed by Foundry | Foundry Agent Service |
| **Custom Agent** | Self-hosted, registered with Foundry control plane | Your infrastructure |
| **Declarative Agent** | Defined via YAML/JSON, low-code | Foundry Agent Service |

## Agent Development Lifecycle

1. **Design** — Define agent purpose, tools, and system message
2. **Build** — Implement using SDK, azd, or declarative YAML
3. **Test** — Use playgrounds and evaluation
4. **Deploy** — Hosted or self-hosted deployment
5. **Publish** — Share within org, to Teams, or to Microsoft 365 Copilot
6. **Monitor** — Trace, evaluate, and iterate

## Development Options

### Option 1: Portal (Low-Code)
- Use the Agent playground at `ai.azure.com`
- Configure system message, tools, and model
- Test interactively before deploying

### Option 2: Azure Developer CLI (`azd`)
```
# Initialize agent from a template
azd ai agent init -m <agent-yaml-url>

# Deploy everything
azd up
```

### Option 3: SDK (Pro-Code)

**Python example pattern:**
```
from azure.ai.projects import AIProjectClient
from azure.identity import DefaultAzureCredential

client = AIProjectClient(endpoint=..., credential=DefaultAzureCredential())
# Create agent, add tools, run conversations
```

Similar patterns exist for JavaScript/TypeScript, C#, and Java.

### Option 4: VS Code Extension
- Declarative workflows (YAML-based, visual editor)
- Pro-code workflows (full SDK integration)
- Built-in debugging and testing

## Agent Tools & Integrations

### Built-in Tools

| Tool | Description | Status |
|------|-------------|--------|
| **Code Interpreter** | Execute Python code in a sandbox | GA |
| **Custom Code Interpreter** | Bring your own runtime | Preview |
| **File Search** | Search over uploaded files (vector store) | GA |
| **Web Search** | Bing-powered web search | Preview |
| **Browser Automation** | Automated browser interaction | Preview |
| **Computer Use** | Desktop interaction via screenshots | Preview |
| **Image Generation** | Create images with DALL-E | Preview |
| **Azure AI Search** | Enterprise search over indexed data | GA |
| **SharePoint** | Search SharePoint content | Preview |
| **Fabric Data Agent** | Query Microsoft Fabric datasets | Preview |
| **Azure Speech** | Speech-to-text and text-to-speech | GA |
| **Azure Language** | NLP tasks (PII detection, language detection) | GA |
| **Azure Translator** | Text and document translation | GA |

### Protocol-Based Integrations

| Protocol | Description | Use Case |
|----------|-------------|----------|
| **MCP (Model Context Protocol)** | Connect external tool servers | Extend agent capabilities with any MCP-compatible server |
| **A2A (Agent-to-Agent)** | Agent communication protocol | Multi-agent orchestration across platforms |
| **OpenAPI** | Define tools from OpenAPI specs | Integrate any REST API as an agent tool |
| **Function Calling** | Define custom functions | Agent invokes your code with structured arguments |

### MCP Integration Options
1. **Connect to existing MCP servers** — Configure in portal or SDK
2. **Build your own MCP server** — Use TypeScript or Python MCP SDKs
3. **MCP authentication** — Configure auth for secured MCP endpoints

> **Tip:** For searching internal documents, also consider **Foundry IQ** (see below) which provides built-in RAG without running your own MCP server. Use MCP when you need custom tool logic or integration with external systems; use Foundry IQ when you primarily need document grounding.

### Tool Governance & Policies
- Route MCP tool calls through an **AI gateway** for rate limiting, auth, logging, and content inspection
- Maintain a curated **tool catalog** of approved tools (built-in and custom)
- Block unauthorized tools; audit tool usage across agents
- Configure AI gateway via portal or Bicep/ARM templates
- See also: GOVERNANCE.md for broader control plane policies

> Live docs: `https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/governance`

### Foundry IQ (RAG)
- **What**: Enterprise knowledge grounding engine
- **How**: Connects agents to knowledge bases automatically
- **Sources**: Azure AI Search, SharePoint, custom connectors
- **Setup**: Connect knowledge base → Foundry IQ indexes and chunks → agents query automatically

## Agent Memory (Preview)

Agents can persist context across conversations:
- **What is memory?** — Agent retains user preferences and context
- **How to use** — Enable memory on the agent, configure retention policies
- **Scope** — Per-user or per-agent memory stores

## Hosted Agent Deployment

### Deploy via Portal
1. Configure agent in the playground
2. Click "Deploy" → choose hosting option
3. Agent gets an endpoint URL

### Deploy via azd
```
azd up  # Provisions infra + deploys agent
azd deploy  # Code-only redeploy
```

### Deploy via CLI
```
az cognitiveservices account project create ...
# Then use SDK to create and configure the agent
```

## Publishing & Sharing

| Destination | How |
|-------------|-----|
| **Within your org** | Publish to internal agent catalog |
| **Microsoft Teams** | Publish as a Teams bot/app |
| **Microsoft 365 Copilot** | Register as a Copilot plugin |
| **Responses API** | Expose agent via standard API protocol |
| **Custom app** | Call agent endpoint from any application |

## System Messages Best Practices

- Design clear system messages that define agent behavior
- Use safety system message templates provided by Microsoft
- Include role definition, constraints, and output format expectations
- Test with adversarial inputs

## Voice Agents (Preview)

- Build voice-enabled agents using Azure Speech integration
- Real-time audio processing via WebRTC or WebSockets
- SIP integration for telephony scenarios

## Live Documentation

- Agent overview: `https://learn.microsoft.com/en-us/azure/foundry/agents/overview`
- Hosted agents: `https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/quickstart-hosted-agent`
- MCP integration: `https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/model-context-protocol`
- Tool catalog: `https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/tool-catalog`
- Agent-to-Agent: `https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/tools/agent-to-agent`
- Foundry IQ: `https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-foundry-iq`
- Publishing: `https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/publish-agent`
