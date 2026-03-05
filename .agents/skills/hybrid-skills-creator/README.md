# hybrid-skills-creator — Skill Documentation

> Human-facing documentation for skill maintainers and reviewers. This file is **not** parsed by the agent.

## Purpose

This skill teaches agents how to create other skills using the hybrid pattern — a rich hub SKILL.md that handles ~80% of tasks inline, with modular reference files for deep dives. It was developed after comparing five architectural approaches (monolith, thin hub, MCP-first, doc index, and hybrid) across several Microsoft Foundry skills.

## Architecture: Hybrid Approach

This skill follows its own advice — it uses the hybrid pattern:

| Component | Content |
|-----------|---------|
| **SKILL.md** (hub) | Decision tree, step-by-step creation flow, validation checklist, troubleshooting, pitfalls |
| **PATTERNS.md** (ref) | Reference file design, cross-referencing, MCP integration, advanced patterns |
| **TEMPLATES.md** (ref) | Copy-paste templates for SKILL.md, reference files, README, frontmatter reference |
| **EVALUATION.md** (ref) | Quality scoring rubric, pattern comparison table, evaluation workflow |

## Key Design Decisions

### 1. Hub contains the full creation workflow
The most common ask is "help me create a skill." The complete step-by-step (scope → hub → references → README → validate) is inline so the agent can work immediately.

### 2. Templates are in a reference, not the hub
Templates are long and only needed when actually writing files. Loading them every time would waste tokens.

### 3. Evaluation checklist is a reference
Scoring and comparison tables are needed for review, not creation. Keeping them separate reduces hub size.

### 4. Derived from empirical comparison
The hybrid pattern and its guidelines come from building and evaluating multiple skill variants (monolith, thin hub, MCP-first, doc index, hybrid) for the same domain (Microsoft Foundry). The hybrid-foundry skill scored highest on self-sufficiency + token efficiency.

## File Inventory

```
hybrid-skills-creator/
├── SKILL.md                  (198 lines) — Hub: creation flow, validation, troubleshooting
├── README.md                 (this file)
└── references/
    ├── PATTERNS.md           (167 lines) — Reference design, cross-refs, MCP, advanced
    ├── TEMPLATES.md          (223 lines) — SKILL.md, reference, README templates
    └── EVALUATION.md         (100 lines) — Quality rubric, pattern comparison, eval workflow
```

**Total**: ~740 lines across 5 files (198 hub + 490 references).

## Maintenance Notes

- Templates should be updated if the SKILL.md frontmatter spec changes (e.g., new required fields).
- The evaluation rubric point values are subjective — adjust based on experience with more skills.
- The pattern comparison table may need updating as new patterns emerge.
