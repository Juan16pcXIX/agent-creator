# What the user already has

Before drawing the tree, find out what exists. A team that ignores the user's own agents, skills and tools will duplicate work and miss the tools they already pay for.

## 1. Agents

Look in, in this order:

1. `.claude/agents/` in the project (recursively)
2. `~/.claude/agents/` (recursively)
3. The agent types listed in this session (they include plugin agents)

For each one write down: `name`, `description`, `model`, and where the file is. Plugin agents cannot be edited (they live in the plugin cache): they can only be kept or left out.

## 2. Skills

Look in `.claude/skills/`, `~/.claude/skills/`, and the skill list of this session. A skill can be:

- **A ready-made workflow** for a leaf (a research skill, a video-editing skill): the leaf preloads it with `skills:` or calls it.
- **An existing orchestrator** for this same root (for example a `/<brand>` skill that already coordinates agents): in that case you are extending a team, not building one. Read it fully and treat its agents as existing.

## 3. Connectors and tools

Check which MCP servers and tools are available in this session (Canva, Notion, Google Drive, Gmail, Slack, a CRM, a design or video tool) and which CLIs the project uses (look at its scripts and docs). Ask only if the work clearly depends on a tool you cannot see: "Which tool do you use to design the posts?"

## 4. Map it onto the tree

Build a table before drafting:

| Existing | Type | Covers | Fits in | Proposal |
|---|---|---|---|---|
| clips19-editor | agent, opus | Builds the final video | production / editor | keep |
| last30days | skill | Recent social media research | marketing / trend-scout | preload in the leaf |
| Canva | connector | Designs | marketing / designer | new leaf that works in Canva |
| old-helper | agent, inherit | Vague, does a bit of everything | nothing clear | leave out, or split |

Proposals for existing agents:

- **keep**: it already does one job well. It joins the team under its current name.
- **upgrade**: it fits a leaf but is weak (no model set, vague description, no output or done-when, does two jobs). Same name, rewritten. The original goes to `skills/<root>/backup/` first.
- **split**: it does two or more jobs. Propose the leaves it should become; the old file stays until the user agrees to retire it.
- **leave out**: it has nothing to do with this root.

The user confirms every one of these in phase 5. Never rename, overwrite or delete an existing agent on your own: other skills or habits may call it by name.

## 5. Show it

In phase 5, the tree marks every node `[exists]`, `[upgrade]` or `[new]`, and lists what did not fit, so the user sees that nothing was ignored.
