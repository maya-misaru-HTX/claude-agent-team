# Knowledge Base Architecture

How this agent team uses curated knowledge bases to give persona-based agents deep domain expertise beyond their training data.

## Our Setup

| Knowledge Base | Size | Files | Agent |
|---------------|------|-------|-------|
| Podcast episode transcripts | 24 MB | 291 episodes | Lenny (Product) |
| Newsletter archive | 7.6 MB | 349 posts | Lenny (Product) |
| Index metadata (JSON) | 920 KB | 1 file | All agents (routing) |
| Episode summaries | 700 KB | 1 file | All agents (quick lookup) |
| Table of contents | 67 KB | 1 file | All agents (navigation) |
| **Total** | **~33 MB** | **643 content files + 3 index files** | |

The knowledge base covers product management, growth, leadership, hiring, AI, design, engineering management, and 80+ other topics from interviews with practitioners at Airbnb, Stripe, Notion, Linear, Figma, Spotify, and hundreds more.

## How It's Organized

```
knowledge/
|-- [source]-transcripts/
|   |-- index/
|   |   |-- episodes.md              # Master episode list
|   |   |-- product-management.md    # 142 episodes tagged
|   |   |-- leadership.md            # 73 episodes tagged
|   |   |-- product-strategy.md      # 52 episodes tagged
|   |   |-- growth-strategy.md       # 33 episodes tagged
|   |   |-- ai.md                    # 27 episodes tagged
|   |   |-- product-led-growth.md    # 23 episodes tagged
|   |   |-- hiring.md                # 19 episodes tagged
|   |   +-- ... (89 topic indexes)
|   +-- episodes/
|       |-- [guest-name]/
|       |   +-- transcript.md
|       +-- ... (291 guest folders)
```

Additionally, a **library** directory provides three derived files for efficient access:

```
docs/
|-- [source]-library/
|   |-- index.json           # Structured metadata: title, guest, date, tags, summary
|   |-- summaries.md         # ~150-word summary per episode
|   +-- table-of-contents.md # Browseable list with agent routing
```

## Index Pattern

The `index.json` file enables structured lookup without reading full transcripts:

```json
{
  "episodes": [
    {
      "id": "[episode-id]",
      "title": "[Episode Title]",
      "guest": "[Guest Name]",
      "date": "2024-01-15",
      "tags": ["product-strategy", "growth", "ai"],
      "summary": "150-word summary of key frameworks and takeaways...",
      "primary_agent": "[agent-name]"
    }
  ]
}
```

## Summary Pattern

Each episode gets a ~150-word summary that captures:
- The guest's main framework or mental model
- 2-3 key tactical takeaways
- When this episode is most useful (what question does it answer?)

```markdown
## [Episode Title] -- [Guest Name]
[Guest] shares their framework for [topic]. Key insight: [one-sentence takeaway].
Tactical advice: (1) [actionable point], (2) [actionable point], (3) [actionable point].
Best for: [when to reference this episode]
```

## Agent Routing

Each episode is tagged with a `primary_agent` field that maps it to the agent most likely to need it:

| Topic Cluster | Agent | Episode Count |
|--------------|-------|---------------|
| Product management, strategy, prioritization | Lenny | 142 |
| Leadership, management, culture | Matt | 73 |
| Growth, marketing, PLG | Kareem | 56 |
| AI, automation, future of work | Nate | 27 |
| Design, UX, craft | Ryo | 15 |
| Hiring, team building | CoS | 19 |

Agents read topic indexes first (`index/[topic].md`), then drill into specific transcripts only when they need the full framework.

## What We Excluded

- **Raw transcripts are local only.** They contain paid subscriber content and are not redistributed.
- **Only indexes and summaries are used for context.** Agents reference summaries to decide which transcript to read, then read the full transcript only when needed.
- **Recent content (last 3 months) is excluded** to respect the paywall window.

## How to Build Your Own

1. **Choose a source.** A podcast, newsletter, book series, or expert blog that aligns with an agent's domain.
2. **Organize by guest/author.** One folder per episode or article. Consistency matters more than cleverness.
3. **Create topic indexes.** Tag each piece with 3-5 topics. Generate index files per topic.
4. **Write summaries.** 150 words per piece. Focus on frameworks and actionable takeaways.
5. **Build a master index.** JSON or markdown with structured metadata for programmatic lookup.
6. **Map to agents.** Tag each piece with the agent most likely to reference it.
7. **Lazy-load everything.** Agents read summaries first, full content only when needed.
