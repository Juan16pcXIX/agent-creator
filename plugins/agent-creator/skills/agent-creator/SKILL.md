---
name: agent-creator
description: Builds or improves a team of Claude Code agents for a project, brand, person or goal. Use when the user asks to "create agents for this", "build me a team of agents", "split this into agents/subagents", "set up departments", types /agent-creator, or wants an existing set of agents or an orchestrator skill extended, made cheaper or more efficient ("my team is missing agents", "add an agent for X", "which model should each agent use"), in any language. Reads the project or conversation and the agents already installed, researches the domain, interviews the user on expertise level and budget, decomposes the work into a tree (root -> department leads -> single-purpose subagents), assigns each agent the cheapest model that can do its job well, and writes the agent files plus an orchestrator skill.
---

# Agent Creator

You turn a project into a working team of Claude Code agents, shaped like a tree:

```
ROOT  (the project, brand, person or goal)  -> orchestrator skill /<slug>
 ├── BRANCH (a department or area)           -> lead agent (optional)
 │    ├── LEAF (one objective, one output)   -> subagent
 │    └── LEAF                               -> subagent
 └── BRANCH
      └── LEAF ...
```

Every agent gets the cheapest model that does its job well. A carousel caption does not need Opus; a two-week content strategy does.

Talk to the user in their own language. Write the generated agents in the language the user picks in the interview.

## The 8 phases

Work through them in order. Do not write any agent file before the user approves the team in phase 5.

### 1. Read the root

Figure out what the root is and what already exists.

- **Inside a project folder**: read `CLAUDE.md`, `README*`, the top-level structure, and any docs or briefs.
- **From the conversation**: use what the user said and what was done in this session. The repeated steps you see in the transcript are the best candidates for leaves.
- **With almost nothing to go on**: ask one open question ("Tell me in two or three sentences what this is and what you want it to achieve") and continue.

Then take an **inventory of what the user already has**, following [references/existing.md](references/existing.md):

- **Agents** in `.claude/agents/` and `~/.claude/agents/` (read each one's `name`, `description` and `model`).
- **Skills** in `.claude/skills/`, `~/.claude/skills/` and the skill list of this session.
- **Connectors and tools** available in this session (MCP servers such as Canva, Notion, Gmail, a CRM; CLIs the project uses).

Match each existing agent to the branch or leaf it would cover. Existing agents are never duplicated, renamed or overwritten without the user's yes. Connectors and skills shape the leaves: if the user has Canva connected, a design leaf works in Canva instead of describing a design in text.

Classify the root with one of the archetypes in [references/archetypes.md](references/archetypes.md) (business/brand, creator, artist, software product, operations, research, personal goal, or custom). The archetype is a starting point, not a template to copy.

### 2. Quick scan (short)

Run 3 to 5 web searches, no more, on how this kind of project is usually run and what tends to be missing. Output a diagnosis of five lines or fewer:

- what the root is, in one line
- what is already covered
- the 2 or 3 biggest gaps or improvements
- the recurring work that would benefit most from agents

This scan is for shaping the tree. The deep research comes later, after the interview.

### 3. Interview

Use AskUserQuestion. Questions and options are in [references/interview.md](references/interview.md). These four need an answer:

1. **Expertise level** of the agents: practical, professional, or expert. This sets how deep phase 6 research goes and how demanding each agent's quality bar is.
2. **Budget**: economical, balanced (recommended), or max quality. This shifts the model routing up or down one tier.
3. **Where to install**: this project (`.claude/agents/`) or all projects (`~/.claude/agents/`).
4. **Language** the agents should work in.

Ask them in one AskUserQuestion call, together with the round-2 choices when the proposal is already on screen, so the user answers once. When the context already settles one (an existing team lives in `~/.claude/agents/` and is written in Spanish), do not ask it: say what you assumed in one line, so the user can correct it. The user may answer in their own words ("quality, but nothing above Opus"): map it to the closest setting and note the limit they gave.

Ask about autonomy only if some agent could spend money, publish, send messages or delete things: the default is that those agents prepare the action and the orchestrator asks the user.

### 4. Draft the tree and route the models

Start from what exists: existing agents take their place in the tree first, and new leaves fill the gaps around them.

Decompose following [references/decomposition.md](references/decomposition.md). The core rules:

- **A leaf has one objective, one kind of output and a clear definition of done.** If you need "and" to describe it, split it.
- **Make a leaf only when the work repeats** (or needs a different model or skill set than its siblings, or needs independent checking). One-off work is done by the lead or the orchestrator.
- **Every branch whose mistakes cost money, reputation or truth gets a verifier leaf** that did not produce the work (fact-checker, QA, reviewer).
- **3 to 7 branches, 2 to 6 leaves per branch.** More than that: merge or split the root.
- **A branch gets a lead agent only when it has 3 or more leaves that need sequencing or planning.** Otherwise its leaves report straight to the orchestrator.

Then score every agent and pick its model with [references/model-routing.md](references/model-routing.md): five dimensions scored 0 to 2 (reasoning depth, ambiguity, cost of error, horizon, judgment), total 0 to 10, mapped to haiku / sonnet / opus (fable only on max-quality budget), then adjusted for budget and volume.

### 5. The user picks the team

The tree you drafted is a proposal. The user decides what goes in. Do it in three steps (questions and formats in [references/interview.md](references/interview.md), round 2):

1. **Show the proposal.** The tree in ASCII with the model and effort next to every node, each node tagged `[exists]`, `[new]` or `[upgrade]`. Then a table: agent, objective, model, effort, why (one short phrase from the score). Then the existing agents and skills that did not fit any branch, so the user sees nothing was ignored.
2. **Let them choose the leaves, branch by branch.** For each branch, one multi-select question listing its candidate leaves (existing ones first, labeled as such, with what they do). The user ticks what they want and uses "Other" to add a leaf you did not think of ("one that makes the Canva templates for the carousels"). Also offer 1 or 2 extra leaves you think could help but did not include, clearly marked as suggestions, so the user can see options beyond your draft.
3. **Settle the existing agents.** For each existing agent that overlaps the tree, ask: keep as is (the team calls it under its current name), upgrade (same name, rewritten with the research and the team's rules, old version backed up), or leave it out of the team.

Every leaf the user adds is treated like the others: give it one objective, score it, route its model, and include it in the research. If what they added is really two jobs, say so and propose the split. If it is a one-off task, say that it does not need an agent and offer to have the orchestrator handle it; if they still want it, add it.

Show the final tree once more and get a yes. Save the approved plan as `team.json` following the schema in [templates/team.schema.json](templates/team.schema.json), including where each agent came from (`existing`, `proposed`, `suggested` or `user`).

### 6. Deep research per branch

Now research each branch at the depth the expertise level asks for (table in [references/research.md](references/research.md)). For professional and expert levels, launch one research subagent per branch in parallel with the Agent tool, `model: "sonnet"`, each with a self-contained brief: the root summary, the branch, its leaves and their objectives, the expertise level, and the output file to write.

Each branch's research ends up in `knowledge/<branch-slug>.md` next to the orchestrator skill: frameworks the pros use, a quality checklist per leaf, common mistakes, the tools or sources that matter, and links. Every claim that matters has a source; anything not verified is marked `[UNVERIFIED]`. The agents read this file instead of carrying it in their prompt, so the prompts stay short and cheap.

### 7. Write the files

Use the templates. Everything for a team with slug `<root>` goes to the install location chosen in phase 3:

| What | Path | Template |
|---|---|---|
| Leaf agents | `agents/<root>-<branch>-<leaf>.md` | [templates/leaf-agent.md](templates/leaf-agent.md) |
| Lead agents | `agents/<root>-<branch>-lead.md` | [templates/lead-agent.md](templates/lead-agent.md) |
| Orchestrator skill | `skills/<root>/SKILL.md` | [templates/orchestrator-skill.md](templates/orchestrator-skill.md) |
| Branch knowledge | `skills/<root>/knowledge/<branch>.md` | written in phase 6 |
| Team map | `skills/<root>/TEAM.md` | [templates/team-map.md](templates/team-map.md) |
| Plan | `skills/<root>/team.json` | [templates/team.schema.json](templates/team.schema.json) |

(`agents/` and `skills/` live under `.claude/` for a project install, or `~/.claude/` for a global install.)

Existing agents:

- **Keep**: do not touch the file. Reference it by its current name in the orchestrator and lead tables.
- **Upgrade**: copy the original to `skills/<root>/backup/<name>.md` first, then rewrite it in place with the same `name`, so whatever already calls it keeps working.
- **Leave out**: do not touch the file and do not reference it.

If the existing agent files are produced by a script (a generator such as `create_agents.py` that rewrites them all), make your changes in that script and run it. Editing the generated files directly would be silently undone the next time the user runs it.

Rules for every new or upgraded agent file:

- `description`: one or two sentences saying **when** to use it and what it hands back. The orchestrator routes by this line, so make it specific. Keep it under 300 characters.
- `model` and `effort` from the routing table.
- `tools`: only what the job needs. Leaves never get `Agent` (add it to `disallowedTools`), so the tree cannot grow by accident. Leads get `Agent` so they can dispatch their leaves.
- Leaves that work through a connector (Canva, Notion, a CRM) or need a skill: either omit `tools` so the agent inherits the session's connectors, keeping `disallowedTools: Agent`, or list the connector's exact tool names. Preload a skill the job depends on with `skills:`. Name the connector or skill in the body so the agent knows to use it.
- The body says: the context in one paragraph, the single objective, inputs it expects, the exact output (file path and format), the definition of done, the quality checklist (from the knowledge file), and what it must never do (invent data, spend money, publish). Write the section headings in the team's language ("Salida", "Está listo cuando"); the validator recognizes the common translations.
- Agents cannot talk to the user (subagents have no AskUserQuestion). They return questions to whoever called them.

### 8. Validate and hand off

Run the validator on what you wrote:

```
python <this-skill-dir>/scripts/validate_team.py <claude-dir> --team <root>
```

`<claude-dir>` is the project's `.claude` folder, or `~/.claude` for a global install. Fix every error it reports, and every warning that points at something real (a missing output or done-when). Then give the user:

- the final tree with models
- how to use it: `/<root>` plus an example request for each branch
- what each run will roughly cost (which tiers fire for a typical request)
- what you could not research or verify
- that new or changed agents load when a session starts: they work from the next session, not this one

## Updating an existing team

If `skills/<root>/team.json` already exists, read it first and work as a diff: add, merge or remove leaves, re-route models, regenerate only the files that changed, and show the user the before and after trees. Never delete an agent file without asking; renaming or removing agents breaks whatever calls them.

If the team was built by hand (an orchestrator skill and agents with a shared prefix, but no `team.json`), treat it the same way: its agents are `existing`, its orchestrator is the one you extend rather than replace, and you write `team.json`, `TEAM.md` and a `backup/` of everything you change at the end.

## What not to do

- Do not create one agent per noun. A "logo agent" that runs once a year is waste; that is a task for the orchestrator.
- Do not give every agent Opus "to be safe". The point of this skill is that each agent costs what its job is worth.
- Do not copy the archetype tree blindly. The user's actual recurring work decides the leaves.
- Do not put the research dump inside agent prompts. Prompts carry the rules; `knowledge/` carries the reference material.
- Do not invent numbers, sources or prices in the research. Mark what you could not check.
