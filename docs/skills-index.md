# Skills Architecture

How this agent team uses skills (slash commands) to encode repeatable workflows as reusable, agent-invokable instructions.

## What Is a Skill?

A skill is a markdown file (`SKILL.md`) that teaches an agent how to execute a specific workflow. When triggered by a slash command (e.g., `/deck-design`) or signal words, the orchestrator loads the skill into the relevant agent's context. Skills turn tribal knowledge into repeatable, consistent execution.

```
skills/
|-- [skill-name]/
|   |-- SKILL.md              # Instructions, steps, quality checks
|   |-- references/            # Supporting docs, templates, rubrics
|   +-- scripts/               # Helper scripts (Python, Node, etc.)
```

## Our Skill Library

48 skills across 8 categories, built over 4 months of daily use.

| Category | Count | Examples | Primary Agent |
|----------|-------|---------|---------------|
| Client Delivery | 12 | intake, report, deck-design, deck-qc, headline, extract-assets, iterate-report, script-builder, survey-json | [Redacted] |
| Engineering | 10 | push, tdd, e2e, refactoring, component, fix-issue, check, review-my-code, explain, pattern | Garry |
| Research | 6 | analysis, narrative, gamma-prompt, report-rubric, non-product-research (x3) | [Redacted] |
| Design | 7 | frontend-design, design-engineer, canvas-design, web-artifacts-builder, algorithmic-art, brand-guidelines, theme-factory | Ryo |
| AI / Automation | 4 | skill-creator, mcp-builder, command, share-skill | Nate |
| Operations | 5 | [manager]-task, [user]-task, follow-up (x3) | CoS |
| Document Processing | 4 | pdf, pptx, docx, xlsx | Shared (any agent) |
| Personal | 3 | cleanup, finder-cleanup, profile-sync | EA |

Additionally, 60+ skills from **gstack** (Garry Tan's open-source engineering suite) are available to the engineering agent.

## SKILL.md Structure

Every skill follows this template:

```markdown
---
description: One-line description of what the skill does and when to trigger it.
  Triggers include "keyword1", "keyword2", "/slash-command".
---

# Skill Name

## Purpose
What this skill produces and why it exists.

## Inputs
What the user provides (file paths, URLs, context, etc.)

## Steps
1. Step-by-step instructions the agent follows
2. Each step should be concrete and actionable
3. Include decision points and branching logic

## Quality Checks
- [ ] Checklist the agent runs before marking complete
- [ ] Each check should be verifiable

## Output
What the agent delivers (file, Notion page, message draft, etc.)

## Anti-Patterns
What NOT to do. Common mistakes to avoid.
```

## Trigger Patterns

Skills are invoked three ways:

1. **Slash commands** -- User types `/deck-design` or `/tdd`. The orchestrator loads the skill directly.
2. **Signal words** -- User says "write a headline" or "create a survey script." The orchestrator matches signal words from the skill's `description` frontmatter.
3. **Agent routing** -- The orchestrator routes to an agent, and the agent recognizes which of its owned skills applies.

## The Compound Skills Flywheel

```
Do it manually
    |
    v
Do it a second time -> "If you do it twice, make it a skill"
    |
    v
Extract into SKILL.md with steps + quality checks
    |
    v
Agent executes consistently every time
    |
    v
Feedback improves the skill -> higher quality each iteration
    |
    v
Share with team (Notion Skill Library)
    |
    v
Compound advantage: every future task is faster
```

The flywheel is real. Our oldest skills (pdf, pptx, docx) have been refined through 50+ uses. Our newest skills start rough and sharpen within 3-5 uses based on feedback saved to memory.

## Skill Design Principles

1. **One skill, one job.** Don't build Swiss Army knives. A deck-design skill should not also do data analysis.
2. **Include quality checks.** Every skill should have a verification step. The agent should never "declare done" without checking its own work.
3. **Reference, don't inline.** Large templates, rubrics, and examples go in `references/`. The SKILL.md stays scannable.
4. **Anti-patterns are as important as patterns.** Documenting what NOT to do prevents the same mistake twice.
5. **Lazy-load references.** Skills should only read reference files when needed, not load everything upfront.
