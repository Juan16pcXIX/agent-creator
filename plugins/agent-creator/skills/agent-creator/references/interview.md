# Interview

Ask with AskUserQuestion, in the user's language. One call can hold up to four questions, so the first round fits in one call. Put the recommended option first.

## Round 1 (before drafting the tree)

**Expertise level**: "How expert should your agents be?"

| Option | Means | Effect |
|---|---|---|
| Professional (recommended) | A solid senior specialist: uses the standard frameworks of the field, checks its own work | Research: 3 to 5 searches per branch, one checklist per leaf |
| Practical | A capable generalist who gets things done fast | Research: 1 or 2 searches per branch, short checklists |
| Expert | Top of the field: cites methods and sources, challenges weak assumptions, explains trade-offs | Research: one research pass per leaf with sources, deeper knowledge files, stricter verifiers |

**Budget**: "How do you want to balance cost and quality?"

| Option | Effect on routing |
|---|---|
| Balanced (recommended) | Scores as computed |
| Economical | Every score minus 1, Opus capped at medium effort |
| Max quality | Scores of 7+ plus 1; the top scores may use Fable |

**Install location**: "Where should the team live?"

| Option | Path |
|---|---|
| This project (recommended when inside a project folder) | `.claude/agents/` and `.claude/skills/<root>/` |
| All my projects | `~/.claude/agents/` and `~/.claude/skills/<root>/` |

**Language**: "What language should the agents work in?" Offer the user's language first, then English, then "Both (bilingual output)".

## Round 2 (after showing the proposed tree)

The user builds the team from your proposal. Ask in this order.

### a. Branches

One multi-select question: "Which areas should the team cover?" Options are the proposed branches (each with a one-line description). The user can untick a branch, or add one with "Other". If they add one, draft its leaves before step b.

### b. Leaves, one question per branch

A multi-select question per branch, up to 4 branches per AskUserQuestion call (it takes at most 4 questions). Header: the branch name.

"For <branch>, which agents do you want?"

Options, in this order:

1. Existing agents that fit, labeled `(you have it)`, with what they do.
2. The leaves you proposed.
3. One or two extra ideas you did not include, labeled `(suggestion)`, so the user sees options beyond your draft.

An AskUserQuestion question holds 2 to 4 options. If a branch has more than 4 candidates, split it into two questions ("<branch>: production" and "<branch>: review and publishing"), or list the extra candidates in the question text and tell the user to name them under "Other".

Always end the question text with: "Missing one? Write it in Other, for example: <a concrete example from this branch>." For marketing with Canva connected, the example could be "one that builds the carousel in Canva from the copy".

Treat everything written under "Other" as new leaves: rewrite each as one objective, check it is one job (split it if not), score it and route its model.

### c. Existing agents that overlap

For each existing agent that fits a leaf, one question: "You already have <name> (<what it does>, <model>). What do we do with it?"

- Keep it as is (recommended when it already does one job well)
- Upgrade it: same name, rewritten with the research and the team rules; the original is backed up
- Leave it out of the team

Recommend "upgrade" first instead when the agent has no model, no clear output, or does several jobs.

### d. Level per branch (optional)

"Should any area be at a different expertise level?" Options: "No, same level for all (recommended)", then the branches as a multi-select for "expert". Ask only when the user's answers or the project suggest it (for example, they care a lot about one area).

### e. Final approval

Show the final tree, marked `[exists]`, `[upgrade]`, `[new]` and `[yours]` (added by the user), with models. "Shall I research and create this team?"

- Yes, create it (recommended)
- Change something

## Only if relevant

**Autonomy**: ask only if a leaf could spend money, publish, send messages, or delete files.

- Prepare and ask me (recommended): the agent leaves everything ready and the orchestrator asks the user before acting.
- Act on its own within limits: ask for the limits (amount, platforms) in the next question.

**Existing tools**: if the work depends on tools (a CRM, an editing app, an API key), ask which ones they already use so the agents are written for those tools and not for generic ones.

## What not to ask

- Anything you can read from the project or the conversation.
- Model choices per agent. You decide them with the routing rules; the user sees them in the tree and can change any of them in step e.
- Questions with a conventional default. Pick the default and mention it.
