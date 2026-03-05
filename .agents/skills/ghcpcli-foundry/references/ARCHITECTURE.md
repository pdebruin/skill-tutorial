# Microsoft Foundry — Architecture & Concepts

## Platform Overview

Microsoft Foundry is a unified, enterprise-grade AI platform that consolidates the entire AI development lifecycle. It was formerly known as **Azure AI Foundry** and rebranded to **Microsoft Foundry** in 2025.

## Core Components

### Foundry Resource (AIServices Account)
- The top-level Azure resource (kind: `AIServices`)
- Contains one or more **Projects**
- Manages billing, networking, and access control
- Created in a specific Azure region

### Projects
- Logical containers within a Foundry resource
- Isolate workloads, agents, model deployments, and data
- Each project has its own endpoint URL
- Team members are granted access per-project via RBAC

### Foundry Models
- A catalog of 1700+ models from multiple providers
- Includes: OpenAI (GPT-5, GPT-4.1, o-series), Meta (Llama), Mistral, Cohere, Anthropic (Claude), Microsoft (Phi), and more
- Models are accessed via **deployments** (named instances with specific SKU/throughput settings)

### Agent Service
- Serverless hosting for AI agents
- Agents can use built-in tools (code interpreter, file search, web search, etc.)
- Supports MCP (Model Context Protocol) and A2A (Agent-to-Agent) protocols
- Agents are published and can be shared across the organization

### Foundry IQ
- Enterprise RAG (Retrieval-Augmented Generation) engine
- Connects agents to organizational knowledge bases
- Uses Azure AI Search, SharePoint, and other connectors under the hood
- Automatic chunking, embedding, and retrieval

### Control Plane
- Centralized governance for agents, models, and tools
- Monitor fleet health, enforce token limits, apply guardrail policies
- Integrates with Microsoft Defender and Entra ID

### AI Services
- Pre-built cognitive services: Speech, Vision, Language, Translation, Content Safety
- Available as standalone APIs or as agent tools within Foundry

## Key Terminology

| Term | Meaning |
|------|---------|
| **Foundry Resource** | Top-level Azure resource (AIServices kind) |
| **Project** | Workspace within a resource for a team or workload |
| **Deployment** | A named instance of a model with specific throughput settings |
| **Model Router** | Automatic routing to the best model for a task based on cost/quality/speed |
| **Hosted Agent** | A serverless agent running on Foundry Agent Service |
| **Capability Host** | Runtime environment that provides tools to an agent |
| **Tool Catalog** | Registry of available tools (built-in and custom) |
| **Guardrail Policy** | Content filtering and safety rules applied to model outputs |
| **PTU** | Provisioned Throughput Unit — reserved capacity for models |

## How Components Connect

```
Azure Subscription
└── Resource Group
    └── Foundry Resource (AIServices)
        ├── Project A
        │   ├── Model Deployments (GPT-5, Llama, etc.)
        │   ├── Agents (hosted, with tools)
        │   ├── Evaluations
        │   └── Connections (to data, APIs, MCP servers)
        └── Project B
            └── ...
```

## Integration Points

- **Azure AI Search** — Vector search, hybrid search for RAG
- **Azure Blob Storage** — File storage for agent data
- **Azure Key Vault** — Secret management for connections
- **Microsoft Entra ID** — Identity and access management
- **Azure Monitor / Application Insights** — Logging and telemetry
- **GitHub / Azure DevOps** — CI/CD for agent and model deployment
- **Microsoft 365 / Teams** — Agent publishing destinations
- **Power Platform** — Low-code integration
