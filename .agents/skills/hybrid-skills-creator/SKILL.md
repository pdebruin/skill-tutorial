---
name: hybrid-skills-creator
description: >-
  Create agent skills using the hybrid pattern — a rich hub SKILL.md that handles common cases
  inline, with modular reference files for deep dives. Use when the user wants to create a new
  agent skill, convert an existing agent skill to hybrid format, or needs guidance on structuring
  an agent skill with reference files. Activate for writing SKILL.md files,
  agent skill architecture, or organizing agent skill content across multiple files.
license: Apache-2.0
metadata:
  author: skill-tutorial
  version: "1.0"
---

# Hybrid Skills Creator

Help users create agent skills using the **hybrid pattern**: a self-sufficient hub SKILL.md backed by modular reference files loaded on demand.

> **Golden rule:** Never generate a SKILL.md without a decision tree routing to references and at least one fully inline workflow for the most common task.

> **Why hybrid?** A monolith skill loads everything every time (token-expensive). A thin hub defers everything to references (slow for common questions). Hybrid handles ~80% of questions inline and defers the rest — the best tradeoff between token cost and self-sufficiency.

## When to Use This Skill

Activate when the user wants to:
- Create a new agent skill from scratch
- Convert an existing skill to hybrid format
- Decide how to split content between hub and references
- Review or improve a skill's architecture

Do **not** activate for general prompt engineering, MCP server development, non-skill agent configuration, or non-agent uses of the word "skill" (e.g., game skill trees, professional skills).

---

## Quick Orientation

An **agent skill** is a folder containing a `SKILL.md` file with YAML frontmatter and markdown instructions that teach an AI agent how to complete a specific task. Skills are the standard way to extend agent capabilities in tools like GitHub Copilot CLI and Claude Code.

| Concept | What It Means |
|---------|--------------|
| **SKILL.md** | The instruction file the agent reads; contains frontmatter (name, description) and markdown body |
| **Frontmatter** | YAML metadata block (`name`, `description`, optional `license`, `allowed-tools`, etc.) |
| **Description** | The trigger — agents decide whether to load a skill based solely on this text |
| **References** | Supporting files the agent reads on demand, loaded via the decision tree |
| **Progressive disclosure** | Loading only what's needed: metadata → hub → references |

---

## The Hybrid Pattern at a Glance

```
my-skill/
├── SKILL.md              ← Hub: orientation, common workflow, guardrails
├── README.md             ← Human docs (not parsed by agent)
└── references/
    ├── TOPIC-A.md        ← Deep dive loaded when decision tree routes here
    ├── TOPIC-B.md        ← Deep dive loaded when decision tree routes here
    └── TOPIC-C.md        ← Deep dive loaded when decision tree routes here
```

**Three-level progressive disclosure:**
1. **Metadata** (name + description) — always in context (~100 words)
2. **Hub** (SKILL.md body) — loaded when skill triggers (<500 lines ideal)
3. **References** — loaded only when the decision tree routes to them

---

## Decision Tree — Is Hybrid Right?

```
How much content does the skill need?

1. Small (< 200 lines total)
   → Monolith is fine. Put everything in SKILL.md.

2. Medium (200–500 lines)
   → Hybrid if there are distinct subtopics. Monolith if it's one linear flow.

3. Large (500+ lines)
   → Hybrid is strongly recommended. No one wants 800 lines loaded every time.

Does the skill cover multiple distinct domains or subtopics?
   YES → Hybrid. Each domain becomes a reference file.
   NO  → Consider monolith if content is short, or split by workflow stage.
```

---

## Creating a Hybrid Skill — Step by Step

### Step 1 — Define Scope and Subtopics

Before writing anything, answer:
1. **What does this skill help with?** (one sentence)
2. **What are the 3–6 distinct subtopics?** (these become reference files)
3. **What's the single most common ask?** (this goes inline in the hub)

### Step 2 — Write the Hub (SKILL.md)

The hub is the heart of the skill. It should be **self-sufficient for the most common case** while routing to references for everything else.

#### Required Hub Sections

| Section | Purpose | Example |
|---------|---------|---------|
| **Frontmatter** | Name, description, metadata | YAML block at top |
| **Title + Golden Rule** | One-line purpose + top behavioral constraint | `> Never invent CLI flags.` |
| **When to Use / Not Use** | Activation triggers and negative scope | Bullet lists |
| **Quick Orientation** | What the technology/domain is (table or short prose) | Pillars table |
| **Decision Tree** | Routes to the right reference file | Numbered or tree format |
| **Inline Workflow** | The most common task, fully inline | Step-by-step with commands |
| **Troubleshooting** | Symptom → resolution table | Reactive fixes |
| **Common Pitfalls** | Mistakes to avoid upfront | Numbered preventive list |
| **Acceptance Criteria** | "Done" conditions for any task | 3–5 concrete checks |
| **Reference File Index** | Links to each reference with when-to-read guidance | List with descriptions |

Not every skill needs every section — adapt to the domain. But the decision tree and inline workflow are what make a hybrid skill work.

#### Hub Writing Principles

- **Self-sufficient for the common case.** If 80% of users ask "how do I set this up?", the full setup flow belongs in the hub — not behind a reference file fetch.
- **Guardrails are always loaded.** Golden rules, acceptance criteria, pitfalls, and troubleshooting go in the hub because they apply regardless of which reference is consulted.
- **Tables over prose** for structured data (commands, roles, symptoms).
- **Numbered lists for sequences**, bullet lists for options.

### Step 3 — Write the Reference Files

Each reference file covers one subtopic in depth. For detailed patterns and templates, read `references/PATTERNS.md`.

Key rules:
- **Defer to the hub** for shared content (don't duplicate CLI commands, auth steps, etc.)
- **Open with a back-reference when there's content overlap** with the hub (e.g., "For initial setup, see SKILL.md Quick Setup."). Self-contained subtopics that the decision tree routes to unambiguously don't need one.
- **Include a Live Documentation section** with URLs specific to that subtopic
- **Keep under 300 lines** if possible; add a table of contents if longer

### Step 4 — Write the README

The README is for human maintainers, not the agent. Document:
- Why you chose the hybrid pattern
- Key design decisions and tradeoffs
- File inventory with line counts
- Maintenance notes (what will go stale, how to refresh)

See `references/TEMPLATES.md` for a full README template.

### Step 5 — Validate

Run through this checklist:
1. ✅ Hub handles the most common ask without loading any reference
2. ✅ Decision tree covers all subtopics and routes clearly
3. ✅ No content is duplicated between hub and references
4. ✅ References that overlap with hub content open with a back-reference
5. ✅ Golden rule / behavioral guardrails are in the hub (always loaded)
6. ✅ Acceptance criteria are concrete and verifiable
7. ✅ Negative scope is defined ("do not activate for...")
8. ✅ Description triggers on likely user phrases and keywords

---

## Troubleshooting

| Symptom | Resolution |
|---------|------------|
| Skill loads too many tokens | Move deep-dive content to references; keep hub under 500 lines |
| Agent doesn't find the right reference | Improve decision tree; make routing criteria explicit |
| Content drifts between hub and references | Establish one source of truth per concept; add cross-references |
| Agent hallucinates when reference is missing | Add fallback: "If unsure, fetch latest via MCP / ask the user" |
| Description doesn't trigger for relevant queries | Make description more inclusive; list specific user phrases |

---

## Common Pitfalls

1. **Duplicating content** — If the hub has CLI setup steps, the reference should say "see SKILL.md" not repeat them.
2. **Too many reference files** — 3–6 is the sweet spot. More than 8 means the decision tree becomes hard to navigate.
3. **Empty hub with everything in references** — That's a thin-hub pattern, not hybrid. The hub must be self-sufficient for common tasks.
4. **Missing decision tree** — Without routing, the agent doesn't know which reference to load. This is the key structural element.
5. **Forgetting negative scope** — Always define what the skill should NOT activate for, to prevent false triggers.
6. **Overloading the description** — Keep it under 200 words. Cover trigger keywords but don't list every feature.

---

## Reference Files (Load Only When Needed)

- [Patterns & Architecture](references/PATTERNS.md) — Reference file design, cross-referencing, MCP integration, advanced patterns
- [Templates](references/TEMPLATES.md) — Copy-paste templates for SKILL.md, reference files, and README
- [Evaluation Checklist](references/EVALUATION.md) — Quality criteria, scoring rubric, comparison with other patterns

---

## Acceptance Criteria

A hybrid skill is "done" when:
1. The hub handles the most common user task fully inline (no reference needed).
2. A decision tree routes all other tasks to the correct reference file.
3. No content is duplicated — each concept has exactly one source of truth.
4. Behavioral guardrails (golden rule, acceptance criteria, pitfalls) are in the hub.
5. The skill passes structural validation (valid frontmatter, required fields present).
