# Microsoft Foundry — Models

## Model Catalog Overview

Microsoft Foundry provides access to 1700+ models from multiple providers, deployable with different throughput and pricing options.

## Model Providers

| Provider | Example Models | Notes |
|----------|---------------|-------|
| **OpenAI** | GPT-5, GPT-4.1, GPT-5-mini, o3, o4-mini | Primary models, deepest integration |
| **Microsoft** | Phi-4, Phi-3.5, Phi-Silica | Small/edge-optimized models |
| **Meta** | Llama 4, Llama 3.3 | Open-weight models |
| **Mistral** | Mistral Large, Mistral Small | European AI provider |
| **Cohere** | Command R+, Embed | Enterprise search/RAG focused |
| **Anthropic** | Claude (via Foundry) | Available in select regions |
| **DeepSeek** | DeepSeek-R1, DeepSeek-V3 | Reasoning models |
| **AI21 Labs** | Jamba | Long-context models |
| **Others** | Various community models | Via Hugging Face integration |

## Deployment Types

| Type | Description | Best For |
|------|-------------|----------|
| **Global Standard** | Shared capacity, pay-per-token | Development, variable workloads |
| **Global Provisioned** | Reserved throughput (PTU) | Production, predictable latency |
| **Standard** | Regional shared capacity | Data residency requirements |
| **Provisioned** | Regional reserved throughput | Compliance + performance |
| **Data Zone Standard** | Shared within a data zone | Data sovereignty |
| **Data Zone Provisioned** | Reserved within a data zone | Sovereignty + performance |

## Model Deployment Options

### Option 1: Portal
1. Go to `ai.azure.com` → Model catalog
2. Select a model → Click "Deploy"
3. Choose deployment type and name
4. Configure throughput (tokens per minute)

### Option 2: Azure CLI
```
az cognitiveservices account model deployment create \
  --account-name <resource-name> \
  --resource-group <rg-name> \
  --deployment-name <deployment-name> \
  --model-id <model-id> \
  --sku <sku>
```

### Option 3: SDKs
All SDKs (Python, JS, C#, Java) support deployment management via the management plane.

### Option 4: Bicep / ARM Templates
Infrastructure-as-code for repeatable, version-controlled deployments.

## Model Router

The **Model Router** automatically selects the best model for each request based on:
- **Quality** — Task complexity and accuracy needs
- **Speed** — Latency requirements
- **Cost** — Token pricing optimization

### How to Use
- Deploy a "model router" deployment instead of a specific model
- The router dynamically picks between configured models per request
- Supports GPT-5 and GPT-4.1 family models

### Configuration Options
- Set quality/speed/cost preferences
- Define which models are eligible for routing
- Monitor routing decisions in the dashboard

## Key APIs for Model Interaction

| API | Use Case |
|-----|----------|
| **Chat Completions** | Standard conversational AI, most common |
| **Responses API** | Newer API with built-in tool support |
| **Embeddings** | Text embedding for search and RAG |
| **Batch** | Async processing at 50% cost |
| **Realtime** | Streaming audio/video via WebRTC or WebSockets |
| **Image Generation** | DALL-E and similar models |
| **Audio** | Whisper (STT), TTS models |

## Fine-Tuning Options

| Technique | Description | When to Use |
|-----------|-------------|------------|
| **Supervised Fine-Tuning** | Train on labeled examples | Improve task-specific accuracy |
| **Vision Fine-Tuning** | Train on image+text pairs | Custom visual understanding |
| **Preference (DPO)** | Train on preference pairs | Align output style/tone |
| **Reinforcement Fine-Tuning** | Reward-based training | Complex behavioral alignment |
| **Distillation** | Compress large model knowledge into smaller one | Cost optimization |

### Fine-Tuning Workflow
1. Prepare training data (JSONL format)
2. Upload dataset to Foundry
3. Create fine-tuning job (portal, CLI, or SDK)
4. Monitor training progress
5. Deploy fine-tuned model
6. Evaluate with built-in safety checks

## Live Documentation

- Model catalog: `https://ai.azure.com/explore/models`
- Deployment guide: `https://learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/deploy-foundry-models`
- Model router: `https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/model-router`
- Fine-tuning: `https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/fine-tuning`
- Deployment types: `https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types`
