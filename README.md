# Claude Agent Team

A real-world, production multi-agent system built on [Claude Code](https://docs.anthropic.com/en/docs/claude-code). This repository contains the configuration files, agent definitions, persona references, and engineering docs that power a team of 8 specialized AI agents orchestrated by a single master agent (Claire).

This is not a demo or tutorial. These files have been used daily to ship code, manage calendars, write insight reports, design interfaces, run growth campaigns, and build new automations. Company-specific content has been redacted with `[placeholders]` so the architecture and patterns can be studied and adapted.

## Why This Exists

A single person with the right AI tooling can operate like a team of twenty. This repo shows how.

The key insight: **specialized agents with clear scope beat general-purpose agents.** Instead of one AI that does everything poorly, this system routes work to the right agent based on signal words, then lets that agent execute with deep domain knowledge, specific MCP tools, and a consistent voice.

## The Team

| Agent | Role | Modeled After |
|-------|------|--------------|
| **Claire** | CPTO / Orchestrator | Claire Vo (ChatPRD, LaunchDarkly) |
| **EA** | Personal Assistant | Ann Hiatt + Bonnie Low-Kramen + Peggy Grande |
| **CoS** | Chief of Staff | Clara Ma + Maggie Olsen |
| **Garry** | Head of Engineering | Garry Tan (YC, gstack) |
| **Ryo** | Head of Design | Ryo Lu (Cursor) |
| **Lenny** | Head of Product | Lenny Rachitsky |
| **Kareem** | Head of Growth | Kareem Amin (Clay) |
| **Nate** | AI Coach | Nate B. Jones |
| **Matt** | Head of Operations | Matt MacInnis (Rippling) |

## The Framework: Delegate / Augment / Human

Every task gets classified into one of three modes:

- **Delegate** -- Fully hand off to an agent. Calendar management, code migrations, report generation, email drafts. The agent owns execution end-to-end.
- **Augment** -- The orchestrator and user work together, with an agent assisting. PRD drafting, code review, design prototyping. The agent provides leverage, the human provides judgment.
- **Human** -- The agent provides context, but the human decides. Customer conversations, strategic decisions, relationship building, feedback that requires empathy.

## Directory Structure

```
claude-agent-team/
|-- README.md                              # You are here
|-- CLAUDE.md                              # Master orchestrator config (Claire)
|-- agents/                                # Agent definitions (8 agents)
|   |-- ea.md                             # Personal Assistant
|   |-- cos.md                             # Chief of Staff
|   |-- garry.md                           # Head of Engineering
|   |-- kareem.md                          # Head of Growth
|   |-- lenny.md                           # Head of Product
|   |-- matt.md                            # Head of Operations
|   |-- nate.md                            # AI Coach
|   +-- ryo.md                             # Head of Design
|-- docs/                                  # Shared knowledge and architecture docs
|   |-- agent-playbook.md                  # Full routing and workflow reference
|   |-- ai-insights-framework.md           # Consumer research methodology
|   |-- skills-index.md                    # Skills architecture and library overview
|   |-- knowledge-base-index.md            # Knowledge base architecture guide
|   +-- memory-system-guide.md             # Memory system architecture guide
+-- docs/agent-personas/                   # Deep persona references
    |-- ea-profile.md                      # EA archetype research
    |-- claire-vo-profile.md               # Claire Vo background
    |-- clara-ma-maggie-olsen-profile.md   # Chief of Staff archetypes
    |-- garry-tan-profile.md               # Garry Tan background
    |-- kareem-amin-profile.md             # Kareem Amin background
    |-- lenny-rachitsky-profile.md         # Lenny Rachitsky background
    |-- matt-macinnis-profile.md           # Matt MacInnis background
    |-- nate-b-jones-profile.md            # Nate B. Jones background
    +-- ryo-lu-profile.md                  # Ryo Lu background
```

## How It Works

### 1. CLAUDE.md is the brain
The `CLAUDE.md` file is loaded into every Claude Code conversation. It defines Claire (the orchestrator), the routing table, compound workflows, permissions, and the self-improvement loop.

### 2. Agents are spawned on demand
When Claire identifies the right agent for a task, she spawns it using Claude Code's Agent tool. Each agent file contains:
- A voice and personality (modeled after a real person)
- Owned skills (slash commands the agent can invoke)
- Key MCP tools the agent uses
- Anti-patterns (things the agent should never do)
- Lazy-loaded context docs (read only when relevant)

### 3. Persona files add depth
Each agent modeled after a real person has a detailed persona reference in `docs/agent-personas/`. These contain background, philosophy, communication style, key quotes, and source material. Agents read these files only when they need deeper voice calibration.

### 4. Docs are shared knowledge
Docs in `docs/` are shared across agents. The Skills Index (`docs/skills-index.md`) documents how skills are structured and triggered. The Knowledge Base Index (`docs/knowledge-base-index.md`) explains how curated content gives agents deep domain expertise. The Memory System Guide (`docs/memory-system-guide.md`) covers how persistent, file-based memory enables continuous improvement. These are loaded lazily to conserve context.

### 5. Compound workflows chain agents
Complex tasks chain multiple agents:
- **New feature e2e:** Lenny (PRD) -> Garry (build) -> Ryo (design review) -> CoS (track progress)
- **Meeting follow-up:** CoS (extract action items) -> EA (schedule follow-ups)
- **New automation:** Nate (evaluate) -> Garry (build) -> Nate (package as skill)

## How to Adapt This

### Start small
You don't need 8 agents. Start with 2-3:
1. An orchestrator (your CLAUDE.md)
2. An engineering agent
3. A domain-specific agent for your core work

### Build persona files from public sources
Every persona in this repo was built from publicly available content: podcasts, blog posts, interviews, conference talks. Pick leaders whose thinking you admire, research their frameworks, and create a persona reference.

### Use the routing table pattern
The routing table in CLAUDE.md is the most important pattern. It maps signal words to agents, so the orchestrator can make instant routing decisions without ambiguity.

### Add skills incrementally
The "if you do it twice, make it a skill" philosophy is real. When you find yourself giving the same instructions repeatedly, extract them into a skill file.

### Lazy-load everything
Agents should only read docs when they need them. Every agent file uses the pattern: "Read X **only when relevant to the current task**." This conserves context window for actual work.

## What's Redacted

Company-specific content has been replaced with placeholders:
- `[User]` -- the person operating the agent team
- `[Manager]` -- the user's manager/CEO
- `[Company]` -- the employer
- `[Company MCP]` -- company-specific MCP server
- `[Client]` -- client names
- `[Project]` -- internal project names
- `[Internal Query Tool]` -- internal data tools
The following are intentionally excluded:
- Skills directory (contains proprietary workflow automations)
- Memory files (contain personal feedback and preferences)
- Company-specific docs (brand guidelines, client profiles, internal protocols)
- Personal profile docs

## Key Principles

1. **Specialized agents beat general-purpose agents.** Give each agent a clear scope, voice, and toolset.
2. **If you do it twice, make it a skill.** Skills compound into a flywheel.
3. **Lazy-load context.** Don't front-load every doc into every conversation.
4. **Self-improvement is not optional.** The system should get better every day based on user feedback.
5. **User sovereignty.** AI recommends, the human decides. Always.
6. **Boil the lake.** When the complete version costs minutes more than the shortcut, do the complete thing.

## Built With

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) by Anthropic
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) for tool integration
- [gstack](https://github.com/AnotherSapiens/gstack) by Garry Tan for engineering workflow skills

## License

This repository is provided as-is for educational purposes. The agent definitions, persona references, and engineering docs are original work. The persona profiles reference publicly available information about real people for the purpose of voice calibration.

---

*Built by a team of one, shipping like a team of twenty.*
