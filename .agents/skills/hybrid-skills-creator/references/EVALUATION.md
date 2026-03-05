# Evaluation Checklist

For the step-by-step creation flow, see SKILL.md. Use this checklist to evaluate whether a hybrid skill is well-constructed.

---

## Quality Scoring Rubric

### Structure (30 points)

| Criterion | Points | How to Check |
|-----------|--------|-------------|
| Valid YAML frontmatter with name + description | 5 | Run `quick_validate.py` |
| Hub under 500 lines | 5 | `wc -l SKILL.md` |
| Decision tree present and routes to all references | 5 | Read the hub |
| 3–6 reference files (not 0, not 12) | 5 | `ls references/` |
| Reference files open with back-reference to hub | 5 | Check first line of each |
| README.md documents architecture decisions | 5 | Read README |

### Self-Sufficiency (30 points)

| Criterion | Points | How to Check |
|-----------|--------|-------------|
| Hub handles the #1 most common task fully inline | 10 | Test with a common prompt without reading refs |
| Behavioral guardrails in hub (golden rule, pitfalls) | 5 | Read the hub |
| Troubleshooting table in hub | 5 | Read the hub |
| Acceptance criteria in hub | 5 | Read the hub |
| Negative scope defined | 5 | "Do not activate for..." present |

### Content Quality (20 points)

| Criterion | Points | How to Check |
|-----------|--------|-------------|
| No content duplicated between hub and references | 5 | Search for repeated text |
| One source of truth per concept | 5 | Check cross-references |
| Commands/code verified against official docs | 5 | Spot-check 2-3 commands |
| Live documentation URLs present and valid | 5 | Click a few links |

### Description Quality (20 points)

| Criterion | Points | How to Check |
|-----------|--------|-------------|
| Triggers on likely user phrases | 5 | Read description, imagine queries |
| Includes keywords users would use | 5 | Check for domain terms |
| Under 200 words / 1024 chars | 5 | Count |
| Negative scope reduces false triggers | 5 | Check for "not for..." language |

### Scoring

| Score | Rating | Meaning |
|-------|--------|---------|
| 90–100 | Excellent | Production-ready |
| 75–89 | Good | Minor improvements needed |
| 60–74 | Adequate | Functional but has gaps |
| < 60 | Needs Work | Significant issues to address |

---

## Comparison: Hybrid vs Other Patterns

Use this when deciding whether an existing skill should be converted to hybrid.

| Pattern | Token Cost | Self-Sufficiency | Maintainability | Best For |
|---------|-----------|-----------------|-----------------|----------|
| **Monolith** | High (everything loaded) | Excellent | Poor (one giant file) | Small skills (< 200 lines) |
| **Thin hub + refs** | Low (hub) + Medium (ref) | Poor (always needs ref) | Good | Content that's rarely needed |
| **MCP-first** | Very low | None (needs network) | Excellent (always fresh) | Volatile content only |
| **Doc index** | Low | None (needs network) | Good | URL catalogs |
| **Hybrid** ✅ | Medium (hub) + On-demand | Good (80% inline) | Good | Most skills with 3+ subtopics |

### When NOT to Use Hybrid

- **Skill is < 200 lines total** — Just use a monolith. The overhead of references isn't worth it.
- **All content is equally important** — If there's no clear "common case" to inline, monolith or thin-hub may work better.
- **Content changes daily** — If everything is volatile, MCP-first with minimal local content is better.
- **Skill does one specific thing** — A focused skill (e.g., "convert CSV to JSON") doesn't need subtopic routing.

### When to Convert to Hybrid

- Existing monolith is over 500 lines
- Users complain the skill is slow or loads too much context
- Skill covers 3+ distinct subtopics with different audiences
- Some content is commonly needed, other content is rarely needed

---

## Running Evaluations

To evaluate a hybrid skill empirically (beyond the checklist), use the skill-creator's evaluation workflow:

1. **Define 2–3 test prompts** covering different subtopics
2. **Run with-skill and without-skill** for each prompt
3. **Grade outputs** against assertions specific to each test
4. **Compare pass rates** — the skill should meaningfully outperform baseline
5. **Check the troubleshooting test** — this is where hybrid skills often shine, since guardrails are always loaded

Key metrics to track:
- **Pass rate delta** — How much better does with-skill perform?
- **Subtopic coverage** — Does the decision tree route correctly for each test?
- **Self-sufficiency** — Can the hub-only tests pass without loading references?
