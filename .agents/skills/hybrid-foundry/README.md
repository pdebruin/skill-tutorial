# hybrid-foundry — Skill Documentation

> Human-facing documentation for skill maintainers and reviewers. This file is **not** parsed by Copilot.

## Purpose

This skill guides Copilot through Microsoft Foundry setup, model deployment, agent development, evaluation, and governance. It's designed to handle the most common tasks inline while providing deep-dive references for specialized topics.

## Architecture: Hybrid Approach

This skill uses a **hybrid** pattern — a rich hub SKILL.md with modular reference files. This was a deliberate choice after comparing four alternative approaches:

| Approach | Tradeoff | Example |
|----------|----------|---------|
| **MCP-first** (thin hub, no inline content) | Low token cost but zero self-sufficiency — useless if MCP tools are unavailable | `foundry/` |
| **Modular hub** (thin hub + refs) | Low activation cost but needs an extra fetch for even basic questions | `microsoft-foundry/` |
| **Monolith** (everything inline) | Self-sufficient but high token cost on every activation; hard to maintain at scale | `ms-foundry/` |
| **Doc index** (curated URL catalog) | Massive URL coverage but zero inline knowledge; every answer requires a network fetch | `azure-microsoft-foundry/` |
| **Hybrid** (rich hub + refs) ✅ | Hub handles ~80% of setup questions inline; refs provide depth on demand | `hybrid-foundry/` |

### Why hybrid wins

- **Self-sufficient for the common case**: Full CLI-first setup flow (auth → resource → project → model → SDK → verify) is inline. No network or reference file needed.
- **Token-efficient for deep dives**: Models, agents, evaluation, governance, and architecture are in reference files — only loaded when the decision tree routes to them.
- **No duplication**: Reference files explicitly defer to SKILL.md for content that lives there (CLI commands, RBAC, auth). Bidirectional cross-references prevent drift.
- **Behavioral guardrails are always loaded**: Golden rule, acceptance criteria, troubleshooting, and pitfalls are in the hub — they apply regardless of which reference file is consulted.

## Key Design Decisions

### 1. Inline Quick Setup over deferred references
The most common ask is "help me set up Foundry." Deferring to a reference file for this adds a round-trip. The full CLI-first path is inlined in SKILL.md so the agent can respond immediately.

### 2. Preconditions checklist before CLI commands
Borrowed from the `foundry/` skill. Four checks (region, RBAC, portal track, enterprise network) prevent wasted provisioning attempts. Placed before Step 1, not buried in troubleshooting.

### 3. Troubleshooting (reactive) separate from Pitfalls (preventive)
Two complementary sections: **Troubleshooting** is a symptom→resolution table for when things break. **Common Pitfalls** is a numbered list of mistakes to avoid upfront. Different mental models, different use cases.

### 4. Golden rule as a top-level callout
> Never invent CLI flags, SDK methods, or URLs.

This is the single most important behavioral constraint. It's a blockquote immediately after the title — impossible for the agent to miss.

### 5. Merged Output Expectations + Acceptance Criteria
Originally two sections with overlapping content ("no hallucinated commands" appeared in both). Merged into a single Acceptance Criteria section with 5 concrete "done" conditions.

### 6. Decision tree with 6 routes (not 7)
"Build an agent" and "Integrate tools" both pointed to AGENTS.md — merged into one entry. Reduces cognitive load for the agent.

### 7. Tool governance consolidated in AGENTS.md
Tool governance (MCP gateway, tool catalog, audit) was split between AGENTS.md and GOVERNANCE.md. Consolidated into AGENTS.md since tools are an agent concern. GOVERNANCE.md focuses on control plane, security, and compliance.

### 8. Reference files defer to SKILL.md (not vice versa)
RESOURCES-AND-SETUP.md and GOVERNANCE.md open with notes like: "For CLI setup, see SKILL.md Quick Setup." This ensures the hub is the single source of truth for shared content (RBAC, auth, CLI commands).

## Good Practices Applied

### Skill design
- **One source of truth per concept** — RBAC roles, auth methods, and CLI commands each live in exactly one place
- **Bidirectional cross-references** — reference files link back to SKILL.md; SKILL.md links to reference files
- **Consistent CLI commands** — MODELS.md deployment command aligned to match SKILL.md's format after catching an inconsistency
- **Explicit negative scope** — "Do not activate for unrelated Azure services or generic cloud questions"

### Content efficiency
- **Tables over prose** for structured data (RBAC, SDK install, troubleshooting, MCP usage)
- **Numbered lists for sequences** (setup steps, preconditions), **bullet lists for options** (activation triggers, pitfalls)
- **No redundant URL tables** — each reference file owns its own "Live Documentation" links; SKILL.md only has the two top-level entry points

### Agent behavior
- **MCP usage table** with concrete tool call examples — the agent knows exactly which tool to call for which scenario
- **Verification step** built into the setup flow — not optional, not a footnote
- **"Fetch before presenting"** philosophy for anything that could go stale (CLI flags, SDK versions, region availability)

## File Inventory

```
hybrid-foundry/
├── SKILL.md                (230 lines) — Hub: orientation, setup, troubleshooting, guardrails
├── README.md               (this file) — Human documentation, not parsed by Copilot
└── references/
    ├── ARCHITECTURE.md      (85 lines) — Platform components, terminology, hierarchy
    ├── RESOURCES-AND-SETUP.md (60 lines) — Portal, azd, Bicep, VS Code, prerequisites
    ├── MODELS.md           (112 lines) — Catalog, deployment types, router, fine-tuning
    ├── AGENTS.md           (164 lines) — Agent types, tools, MCP, governance, publishing
    ├── EVALUATION-AND-OBSERVABILITY.md (130 lines) — Evaluators, tracing, monitoring, CI/CD
    └── GOVERNANCE.md       (108 lines) — Control plane, security, compliance, cost
```

**Total**: 889 lines across 7 files (230 hub + 659 references).

## Maintenance Notes

- **SDK versions** in SKILL.md Step 4 will need updating as packages move from preview to GA.
- **CLI commands** should be verified against `microsoft_docs_fetch` periodically — flags may change.
- **Reference files** can be updated independently; just ensure cross-references remain valid.
- Consider adding the `generated_at` metadata pattern (from `azure-microsoft-foundry`) to track freshness.
