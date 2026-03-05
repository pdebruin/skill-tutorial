# Templates

For the step-by-step creation flow, see SKILL.md. These templates are starting points — adapt them to your domain.

---

## SKILL.md Template

````markdown
---
name: my-skill-name
description: >-
  One-paragraph description of what this skill does and when to trigger it.
  Include specific user phrases and keywords that should activate this skill.
  Be slightly "pushy" — list contexts where activation is appropriate even if
  the user doesn't explicitly name the skill.
license: Apache-2.0
metadata:
  author: your-name
  version: "1.0"
---

# Skill Title

One-sentence purpose statement.

> **Golden rule:** [The single most important behavioral constraint for this skill.]

## When to Use This Skill

Activate when the user wants to:
- [Task 1]
- [Task 2]
- [Task 3]

Do **not** activate for [negative scope].

---

## Quick Orientation

[Brief overview of the domain — a table of key concepts or a 2-3 sentence summary.]

| Concept | What It Does |
|---------|-------------|
| **Concept A** | ... |
| **Concept B** | ... |

---

## Decision Tree — What Do You Need?

```
1. [Most common task]
   → Follow the Quick Setup below

2. [Subtopic A]
   → Read references/SUBTOPIC-A.md

3. [Subtopic B]
   → Read references/SUBTOPIC-B.md

4. [Subtopic C]
   → Read references/SUBTOPIC-C.md
```

---

## Quick Setup (Handles 80% of Cases)

### Preconditions
1. [Check 1]
2. [Check 2]

### Step 1 — [First action]
```bash
# command here
```

### Step 2 — [Second action]
```bash
# command here
```

### Step 3 — Verify
[Verification step — run a command, check output, confirm success.]

---

## Troubleshooting

| Symptom | Resolution |
|---------|------------|
| [Error 1] | [Fix 1] |
| [Error 2] | [Fix 2] |

---

## Common Pitfalls

1. **[Pitfall 1]** — [Explanation and how to avoid]
2. **[Pitfall 2]** — [Explanation and how to avoid]

---

## Acceptance Criteria

A task is "done" when:
1. [Criterion 1]
2. [Criterion 2]
3. [Criterion 3]

---

## Reference Files (Load Only When Needed)

- [Subtopic A](references/SUBTOPIC-A.md) — [One-line description]
- [Subtopic B](references/SUBTOPIC-B.md) — [One-line description]
- [Subtopic C](references/SUBTOPIC-C.md) — [One-line description]
````

---

## Reference File Template

```markdown
# Subtopic Name

For [shared concept like setup, auth, CLI commands], see SKILL.md [section name].

---

## Core Concepts

[Key information for this subtopic — tables, definitions, architecture.]

## How-To

### [Procedure 1]
[Step-by-step instructions.]

### [Procedure 2]
[Step-by-step instructions.]

## Advanced / Edge Cases

[Less common scenarios, configuration options, known limitations.]

---

## Live Documentation

| Resource | URL |
|----------|-----|
| Official docs | `https://...` |
| API reference | `https://...` |
```

---

## README Template

````markdown
# my-skill-name — Skill Documentation

> Human-facing documentation for skill maintainers and reviewers.
> This file is **not** parsed by the agent.

## Purpose

[One paragraph on what this skill does and who it's for.]

## Architecture: Hybrid Approach

This skill uses a **hybrid** pattern — a rich hub SKILL.md with modular reference files.

| Approach | Tradeoff |
|----------|----------|
| **MCP-first** | Low token cost but zero self-sufficiency |
| **Modular hub** | Low activation cost but needs extra fetch for basics |
| **Monolith** | Self-sufficient but high token cost every time |
| **Hybrid** ✅ | Hub handles ~80% inline; refs provide depth on demand |

## Key Design Decisions

### 1. [Decision]
[Why you made this choice.]

### 2. [Decision]
[Why you made this choice.]

## File Inventory

```
my-skill-name/
├── SKILL.md              (N lines) — Hub: [summary]
├── README.md             (this file)
└── references/
    ├── SUBTOPIC-A.md     (N lines) — [summary]
    ├── SUBTOPIC-B.md     (N lines) — [summary]
    └── SUBTOPIC-C.md     (N lines) — [summary]
```

**Total**: N lines across M files.

## Maintenance Notes

- [What will go stale and how to refresh it.]
- [Cross-references to keep in sync.]
````

---

## Frontmatter Field Reference

| Field | Required | Description |
|-------|----------|-------------|
| `name` | ✅ | Kebab-case identifier, max 64 chars |
| `description` | ✅ | When to trigger + what it does, max 1024 chars |
| `license` | | SPDX identifier (e.g., `Apache-2.0`, `MIT`) |
| `compatibility` | | Required tools, dependencies, versions |
| `metadata` | | Custom key-value pairs (author, version, etc.) |
| `allowed-tools` | | Space-separated list of MCP tools the skill may use |
