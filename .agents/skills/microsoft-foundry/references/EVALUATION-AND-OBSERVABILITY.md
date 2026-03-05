# Microsoft Foundry — Evaluation & Observability

## Overview

Microsoft Foundry provides built-in tools for evaluating AI agent quality, tracing execution, and monitoring production systems. This covers the full lifecycle from development testing to production observability.

## Evaluation

### Built-in Evaluator Categories

| Category | Examples | Use Case |
|----------|---------|----------|
| **General Purpose** | Coherence, fluency, relevance | Overall output quality |
| **Textual Similarity** | BLEU, ROUGE, F1 | Compare against reference text |
| **RAG Evaluators** | Groundedness, context relevance, answer completeness | Evaluate retrieval-augmented generation |
| **Risk & Safety** | Violence, self-harm, sexual content, hate | Content safety checks |
| **Agent Evaluators** | Task completion, tool usage accuracy | Agent-specific quality metrics |
| **Azure OpenAI Graders** | Custom prompt-based graders | Model-as-judge evaluation |
| **Custom Evaluators** | User-defined metrics | Domain-specific quality checks |

### Evaluation Options

| Method | How | Best For |
|--------|-----|----------|
| **Portal** | Run evaluations in the Foundry UI | Quick, interactive evaluation |
| **SDK** | `azure-ai-projects` SDK | Programmatic, repeatable evaluation |
| **GitHub Actions** | CI/CD pipeline integration | Automated quality gates |
| **Azure DevOps** | Pipeline task | Enterprise CI/CD |

### Evaluation Workflow
1. Prepare test dataset (input/expected output pairs)
2. Select evaluators (built-in or custom)
3. Run evaluation (portal, SDK, or CI/CD)
4. Review results in the portal dashboard
5. Compare across runs and model versions
6. Set quality thresholds for automated gating

## AI Red Teaming

### What It Is
- Automated adversarial testing of AI models and agents
- Probes for vulnerabilities, biases, and harmful outputs
- Built-in red teaming agent that generates attack scenarios

### Options
| Method | Description |
|--------|-------------|
| **Cloud scans** | Run red teaming in Foundry (no local compute needed) |
| **Local scans** | Run red teaming locally for sensitive scenarios |
| **Custom scenarios** | Define your own adversarial test cases |

## Tracing

### Agent Tracing
- Track every step of agent execution (model calls, tool invocations, decisions)
- Integrates with Azure Monitor and Application Insights
- Supports OpenTelemetry-compatible exporters

### Setup Options
| Method | How |
|--------|-----|
| **Portal** | Enable tracing on agent configuration |
| **SDK** | Add tracing instrumentation to code |
| **Framework integrations** | Built-in tracing for Semantic Kernel, LangChain, etc. |

### What Gets Traced
- Model inference calls (input, output, latency, tokens)
- Tool invocations (which tool, arguments, results)
- Agent decision flow (routing, handoffs)
- Error states and retries

## Monitoring

### Dashboard Options

| Dashboard | What It Shows |
|-----------|--------------|
| **Agent Dashboard** | Agent-level metrics (sessions, success rate, latency) |
| **Model Monitoring** | Deployment metrics (requests, tokens, errors, latency) |
| **Optimization Dashboard** | Cost and performance insights with AI recommendations |
| **Fleet Health** | Cross-agent/cross-model overview (Control Plane) |

### Key Metrics to Monitor
- **Latency** — Response time per model call and end-to-end
- **Token usage** — Input/output tokens per request
- **Error rate** — Failed requests, timeouts, content filter blocks
- **Cost** — Per-deployment and per-project spending
- **Quality** — Evaluation scores over time (via continuous evaluation)

### Alerting
- Configure alerts via Azure Monitor
- Set thresholds for latency, error rate, cost
- Integrate with PagerDuty, Slack, email, etc.

## Cluster Analysis

- Group evaluation results by topic or error type
- Identify systematic failure patterns
- Prioritize improvements based on cluster size and severity

## Human Evaluation

- Route uncertain or high-stakes outputs to human reviewers
- Integrate human feedback into evaluation pipelines
- Track inter-rater agreement for quality assurance

## CI/CD Integration

### GitHub Actions
```yaml
# Example: Run evaluation as a quality gate in CI
- uses: azure/ai-evaluation-action@v1
  with:
    project-endpoint: ${{ secrets.PROJECT_ENDPOINT }}
    evaluation-config: ./eval-config.yaml
```

### Azure DevOps
- Pipeline task for running evaluations
- Quality gates based on evaluation scores
- Artifact publishing for evaluation reports

## Live Documentation

- Observability overview: `https://learn.microsoft.com/en-us/azure/foundry/concepts/observability`
- Built-in evaluators: `https://learn.microsoft.com/en-us/azure/foundry/concepts/built-in-evaluators`
- Agent tracing: `https://learn.microsoft.com/en-us/azure/foundry/observability/concepts/trace-agent-concept`
- Red teaming: `https://learn.microsoft.com/en-us/azure/foundry/concepts/ai-red-teaming-agent`
- CI/CD evaluations: `https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action`
- Human evaluation: `https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/human-evaluation`
