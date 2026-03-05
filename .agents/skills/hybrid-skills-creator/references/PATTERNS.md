# Reference File Patterns & Architecture

For initial setup and the step-by-step creation flow, see SKILL.md.

---

## Reference File Design

### Anatomy of a Good Reference File

```markdown
# Topic Name

For [shared concept], see SKILL.md [section name].

---

## Section 1 — Core Concepts
(tables, definitions, key information)

## Section 2 — How-To
(step-by-step procedures specific to this subtopic)

## Section 3 — Advanced / Edge Cases
(less common scenarios)

---

## Live Documentation

| Resource | URL |
|----------|-----|
| Official docs | `https://...` |
| API reference | `https://...` |
```

### Key Rules

1. **Open with a back-reference when there's overlap** — If the reference touches content also in the hub (setup steps, auth, shared concepts), the first line after the title should point back. Self-contained subtopics routed unambiguously by the decision tree don't need one.

2. **Own your subtopic completely** — Everything about this subtopic lives here. The hub should not contain deep details that belong in a reference.

3. **Keep under 300 lines** — If longer, add a table of contents at the top. If much longer (500+), consider splitting into two references.

4. **End with live documentation links** — URLs specific to this subtopic. The hub only has top-level entry point URLs.

---

## Cross-Referencing Patterns

### Hub → Reference (routing)

In the hub's decision tree:
```markdown
3. Build an AI agent or integrate tools
   → Read references/AGENTS.md
```

In the hub's reference index:
```markdown
- [Agents](references/AGENTS.md) — Agent development, hosting, tools, publishing
```

### Reference → Hub (deferring)

At the top of a reference file:
```markdown
For CLI setup commands and RBAC roles, see SKILL.md Quick Setup.
```

Within a reference, when referencing hub content:
```markdown
Use the deployment command from SKILL.md Step 3, substituting the model name.
```

### Reference → Reference (rare, use sparingly)

```markdown
For tool governance policies, see GOVERNANCE.md.
```

Only cross-reference between references when there's a clear dependency. Most references should be independent.

---

## MCP Integration Pattern

When a skill works with MCP tools (e.g., documentation search), include a usage table in the hub:

```markdown
## Using MCP to Stay Current

| Action | Tool call |
|--------|-----------|
| Find CLI commands | `microsoft_docs_search(query="...")` |
| Get full page | `microsoft_docs_fetch(url="...")` |
| Find code samples | `microsoft_code_sample_search(query="...", language="...")` |
```

This gives the agent concrete examples of how to use the tools, reducing hallucinated tool calls.

If the skill declares `allowed-tools` in frontmatter, only list tools the skill actually uses:
```yaml
allowed-tools: microsoft_docs_search microsoft_docs_fetch microsoft_code_sample_search
```

---

## When Content Should Live in Hub vs Reference

| Content Type | Hub | Reference | Why |
|-------------|-----|-----------|-----|
| Auth / login steps | ✅ | | Used by almost every task |
| Quick start / setup | ✅ | | Most common ask |
| Troubleshooting table | ✅ | | Applies to all tasks |
| Common pitfalls | ✅ | | Preventive, always relevant |
| Acceptance criteria | ✅ | | Behavioral guardrail |
| Golden rule | ✅ | | Must always be loaded |
| Domain deep-dive | | ✅ | Only needed for specific tasks |
| Advanced configuration | | ✅ | Rarely needed |
| API reference details | | ✅ | Verbose, domain-specific |
| Live doc URLs (subtopic) | | ✅ | Scoped to one subtopic |

**Rule of thumb:** If removing it from the hub would break >50% of tasks → keep in hub. If it's only needed for <20% of tasks → put in a reference.

---

## Advanced Patterns

### Variant-Based References

When a skill supports multiple platforms/languages/frameworks, organize references by variant:

```
cloud-deploy/
├── SKILL.md (common workflow + selection logic)
└── references/
    ├── AWS.md
    ├── GCP.md
    └── AZURE.md
```

The decision tree routes based on the user's platform choice. Only one reference gets loaded.

### Layered References

When subtopics have sub-subtopics, you can nest (but keep it shallow):

```
complex-skill/
├── SKILL.md
└── references/
    ├── MODELS.md          (covers model selection, deployment, fine-tuning)
    ├── AGENTS.md          (covers agent types, tools, publishing)
    └── EVALUATION.md      (covers evaluators, tracing, monitoring)
```

Each reference handles its own sub-sections internally. Don't create `references/models/deployment.md` — that's too deep. Keep the hierarchy to two levels: hub → reference.

### Snapshot + MCP Hybrid

For fast-moving domains, store stable content locally and fetch volatile content via MCP:

- **Local (SKILL.md + references):** Architecture, patterns, best practices, workflow steps
- **MCP (fetched on demand):** CLI flag names, SDK versions, region availability, pricing

Add a golden rule like: "Never invent CLI flags or SDK methods. When in doubt, fetch the latest from docs."
