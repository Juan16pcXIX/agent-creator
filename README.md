# agent-creator

**Give it a project. Get back a team of Claude Code agents, each on the cheapest model that can do its job well.**

[Leer en español](README.es.md)

agent-creator is a skill for [Claude Code](https://code.claude.com). Point it at a project folder, a conversation, or a short description ("a streetwear brand", "I make music", "our logistics paperwork"), and it:

1. **Reads the root**: what the project is, what work keeps repeating, and what you already have (your agents, skills and connectors such as Canva or Notion).
2. **Scans the field briefly**: how this kind of project is usually run and what is missing.
3. **Interviews you**: how expert the agents should be, your budget, where to install them, what language they work in.
4. **Draws a tree**: departments (branches) and single-purpose subagents (leaves), with a verifier wherever mistakes are expensive.
5. **Routes every agent to a model** with a 10-point rubric: Haiku for mechanical work, Sonnet for most production work, Opus for strategy and high-stakes calls.
6. **Lets you pick the team**, department by department: tick the agents you want, keep or upgrade the ones you already have, and add any agent it did not think of ("one that builds the carousels in Canva").
7. **Researches each department** at the depth you chose and writes it into knowledge files the agents read.
8. **Writes the agent files and an orchestrator skill** (`/your-project`) that routes requests to the right agent, and **validates the result** with a bundled script.

```
norte  (/norte)
├── marketing  ── norte-marketing-lead  [sonnet · medium]
│   ├── strategist        [opus · high]     2-4 week content and campaign plan
│   ├── trend-scout       [haiku · medium]  this week's trends, with links
│   ├── carousel-writer   [sonnet · medium] one carousel from a brief
│   └── fact-checker      [sonnet · medium] checks claims, prices, dates
└── finance  (no lead: only two leaves)
    ├── bookkeeper           [haiku · low]     categorizes transaction exports
    └── cashflow-forecaster  [opus · medium]   13-week forecast, risky weeks
```

*A real output for a fictional clothing brand. All files are in [examples/clothing-brand](examples/clothing-brand/.claude).*

## Why

Building a team of agents by hand takes hours, and most people end up with one of two bad teams: a few giant agents that do everything badly, or every agent on the most expensive model "to be safe". A Haiku call costs about a twentieth of a Sonnet call. If the mechanical half of your work runs on Haiku and only the strategy runs on Opus, the team does the same work for a fraction of the tokens.

## Install

**As a plugin** (recommended, gets updates):

```
/plugin marketplace add Juan16pcXIX/agent-creator
/plugin install agent-creator@agent-creator
```

**By hand**: copy `plugins/agent-creator/skills/agent-creator` into `~/.claude/skills/` (all projects) or `.claude/skills/` (one project).

## Use

In Claude Code, inside your project:

```
/agent-creator
```

or just ask: "create a team of agents for this project", "split my brand into departments with agents". You can also describe a root that has no folder yet: "build me agents for my music career: I write, record vocals at home and release a single every 6 weeks."

When it finishes you get `/your-project`, an orchestrator skill that knows the whole team, plus the agent files in `.claude/agents/`. Run `/agent-creator` again later to add, merge or remove agents: it reads the saved `team.json` and works as a diff.

## How models are chosen

Each agent is scored 0 to 2 on five things: reasoning depth, ambiguity, cost of an error, horizon, and judgment. The total (0 to 10) maps to a model:

| Score | Model | For |
|---|---|---|
| 0-4 | Haiku | Files, formatting, lookups, data entry, checklists |
| 5-7 | Sonnet | Writing from a brief, coding, research, fact-checking |
| 8-10 | Opus | Multi-week plans, strategy, money and high-stakes decisions |

Then it adjusts: your budget moves everything up or down a tier, agents that run many times per request drop a tier, verifiers are never weaker than what they check, and anything an audience reads is at least Sonnet. Fable is used only if you choose "max quality". Full rubric: [model-routing.md](plugins/agent-creator/skills/agent-creator/references/model-routing.md).

## How the tree is cut

- A **leaf** does one recurring job with one output and a checkable "done when". One-off tasks don't get agents.
- Every department where mistakes cost money, reputation or truth gets a **verifier** that did not produce the work.
- A **lead** agent exists only when a department has 3+ agents that need planning or sequencing. Otherwise the orchestrator calls them directly, which is cheaper.
- 3 to 7 departments, 2 to 6 agents each.

Details and archetypes (business, creator, musician, software, logistics, research, personal goals): [references/](plugins/agent-creator/skills/agent-creator/references).

## What's in the repo

```
.claude-plugin/marketplace.json        marketplace entry
plugins/agent-creator/
  .claude-plugin/plugin.json
  skills/agent-creator/
    SKILL.md                           the 8 phases
    references/                        decomposition, model routing, interview, research, archetypes
    templates/                         leaf agent, lead agent, orchestrator skill, team map, team.json schema
    scripts/validate_team.py           checks a generated team (stdlib Python)
examples/
  clothing-brand/                      a full generated team that passes validation
  musician/                            a team map, in Spanish
```

Check a generated team yourself:

```
python plugins/agent-creator/skills/agent-creator/scripts/validate_team.py examples/clothing-brand/.claude --team norte
```

## Roadmap

- [ ] More examples (software startup, logistics office, YouTube channel)
- [ ] Cost estimate per typical request, measured on real runs
- [ ] Evals: a fixed set of project descriptions and a rubric for the trees they should produce
- [ ] Ready-made team packs per industry

Ideas and pull requests are welcome. Open an issue with the kind of project you tried it on and the tree you got.

## License

[MIT](LICENSE)
