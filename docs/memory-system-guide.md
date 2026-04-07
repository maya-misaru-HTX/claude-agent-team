# Memory System Architecture

How this agent team uses persistent, file-based memory to maintain context across conversations and improve over time.

## Our Setup

| Metric | Value |
|--------|-------|
| Total memory files | 18 |
| Memory types | 4 (user, feedback, project, reference) |
| Index file | MEMORY.md (~25 entries, one-line pointers) |
| Avg file size | ~800 bytes |
| Oldest memory | March 2026 |

Memory is what makes the agent team compound. Without it, every conversation starts from zero. With it, corrections stick, preferences persist, and the system gets sharper every day.

## Memory Types

### User Memories
Information about the user's role, goals, preferences, and working style. Helps agents tailor their behavior.

```markdown
---
name: user_mbti_istp
description: [User]'s MBTI type and what it means for collaboration
type: user
---

[User] is ISTP (Virtuoso). Discovered during coaching session (March 2026).
Key implications: values autonomy, prefers practical action over theory,
communicates directly, learns by doing not discussing.
```

### Feedback Memories
Corrections and confirmations from the user about how agents should work. The most important type -- prevents the same mistake twice.

```markdown
---
name: feedback_no_emdash
description: Avoid em-dashes in writing -- biggest AI tell
type: feedback
---

Don't use em-dashes unless absolutely necessary.
**Why:** Em-dashes are the biggest AI writing tell. Makes all output
feel generated rather than human-written.
**How to apply:** Use commas, periods, or parentheses instead.
Applies to all written output across all agents.
```

### Project Memories
Information about ongoing work, goals, and initiatives that isn't derivable from code or git history.

```markdown
---
name: project_archived_codebases
description: Codebases that are archived -- reference only, do not modify
type: project
---

[Project A], [Project B], [Project C] are archived.
Reference only -- do not attempt to build, deploy, or modify.
```

### Reference Memories
Pointers to where information lives in external systems.

```markdown
---
name: reference_mcp_port
description: Stale process on port [XXXXX] causes MCP reconnect failure
type: reference
---

When [Company MCP] fails to connect, check for stale process on port [XXXXX].
Kill it to fix: lsof -ti:[XXXXX] | xargs kill -9
```

## File Naming Convention

```
{type}_{topic}.md
```

Examples:
- `user_mbti_istp.md`
- `feedback_no_emdash.md`
- `feedback_concise_bullets.md`
- `project_archived_codebases.md`
- `reference_mcp_port.md`

Type prefix makes it easy to scan by category. Topic suffix is descriptive enough to understand without opening the file.

## MEMORY.md Index

The index file is a flat list of one-line pointers. It's loaded into every conversation, so it must stay concise (<200 lines). Each entry links to the full memory file.

```markdown
- [MBTI is ISTP](user_mbti_istp.md) -- Virtuoso type; autonomy, practical action, direct communication
- [Avoid em-dashes](feedback_no_emdash.md) -- don't use unless absolutely necessary; biggest AI writing tell
- [Prefer CSV over MCP](feedback_csv_over_mcp.md) -- calculate from CSV when available; MCP approval prompts slow workflow
- [Archived codebases](project_archived_codebases.md) -- [Project A], [Project B] are archived; reference only
- [MCP port fix](reference_mcp_port.md) -- stale process on port causes reconnect failure; kill it to fix
```

Rules:
- One line per entry, under ~150 characters
- Format: `- [Title](file.md) -- one-line hook`
- No frontmatter in MEMORY.md -- it's an index, not a memory
- Organized semantically by topic, not chronologically

## When to Save vs. Not Save

### Save
- User corrections: "don't do that," "stop doing X" -> feedback memory
- User confirmations of non-obvious approaches: "yes, exactly" -> feedback memory
- User context: role, preferences, working style -> user memory
- Project context not in code: who's doing what, why, deadlines -> project memory
- External system pointers: where bugs are tracked, what dashboard to check -> reference memory

### Do Not Save
- Code patterns or architecture (derivable from reading the codebase)
- Git history or recent changes (use `git log` / `git blame`)
- Debugging solutions (the fix is in the code; the commit message has context)
- Anything already in CLAUDE.md or agent files
- Ephemeral task details only useful in the current conversation

## Memory Lifecycle

```
Observe (user correction or confirmation)
    |
    v
Save (write memory file + add to MEMORY.md index)
    |
    v
Apply (use in future conversations to guide behavior)
    |
    v
Verify (before acting on a memory, check it's still accurate)
    |
    v
Update or Retire (if stale, fix it; if obsolete, remove it)
```

**The bar:** If the user gives the same feedback twice, the system failed. The first correction should have been saved and applied permanently.
