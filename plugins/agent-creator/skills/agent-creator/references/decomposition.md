# Decomposing a root into branches and leaves

## The three levels

| Level | Is | Becomes | Talks to |
|---|---|---|---|
| Root | The project, brand, person, system or goal | Orchestrator skill `/<slug>`, runs in the main conversation | The user, and every lead or leaf |
| Branch | An area with its own expertise: a department (marketing, finance), a craft (vocals, mixing), a stage (intake, review) | A lead agent, or just a group of leaves | The orchestrator, and its own leaves |
| Leaf | One recurring job with one output | A subagent | Whoever called it. Never the user. |

Claude Code lets a subagent spawn subagents up to three layers below the main conversation. This skill uses at most two: orchestrator -> lead -> leaf. A third layer adds latency and context cost for little gain.

## How to find the branches

Pick the cut that matches how the work actually flows. Try these lenses and keep the one with the fewest overlaps:

1. **By function** (who does it in a real organization): marketing, sales, finance, operations, product, support. Best for businesses and brands.
2. **By craft** (which skill it needs): for a musician, writing, vocals, production, mixing and mastering, release and promotion. Best for artists and makers.
3. **By stage** (where in the pipeline): intake, draft, review, publish. Best for processes such as logistics, publishing or document review.
4. **By outcome** (for a personal goal): for "run a marathon in 6 months", training plan, nutrition, recovery, tracking.

A branch must be something the user would recognize and name themselves. If you need to explain it, the cut is wrong.

## How to find the leaves

List every task the branch does. For each one ask:

| Question | If yes |
|---|---|
| Does it repeat (weekly, per product, per piece of content, 3+ times per project)? | Candidate leaf |
| Does it need a very different model or skill set than its siblings? | Separate leaf, even if rare |
| Would the result be better if someone other than the author checked it? | Add a verifier leaf |
| Is it a one-off (logo, company registration, initial setup)? | Not a leaf. The lead or orchestrator does it |
| Does it need to talk to the user to be done? | Not a leaf. Leaves cannot talk to the user; the orchestrator does that part |

Then merge or split:

- **Split** a leaf whose description needs "and" between two different outputs ("researches trends and writes the script" is two leaves).
- **Merge** two leaves that always run together, on the same input, at the same model tier, and never separately.

## Verifiers

A verifier checks work it did not produce. Add one to every branch where an error is expensive:

- Marketing: a fact-checker that verifies every claim, number and quote has a source.
- Finance: a reconciler that re-adds the figures from the source data.
- Code: a reviewer or tester.
- Logistics or legal documents: a second reviewer reading against a checklist, looking from a different angle than the first (for example one checks quantities and dates, the other checks names, addresses and terms).

The verifier returns PASS or a list of concrete problems. It does not fix the work itself; the author does, so the author learns and the check stays independent.

## Leads: when a branch needs one

Give a branch a lead agent only if at least one is true:

- It has 3 or more leaves that must run in a specific order or in parallel.
- Its work needs a plan before the leaves start (a campaign, a release, a sprint).
- Someone has to make the final call between options the leaves produce.

Otherwise skip the lead. The orchestrator calls the leaves directly, which is cheaper and faster.

## Size limits

- 3 to 7 branches. Fewer than 3: the project may not need a team, just 2 or 3 agents. More than 7: merge branches or split the root into two teams.
- 2 to 6 leaves per branch.
- Total agents above about 25: warn the user. Descriptions of all agents load into every session, and Claude Code warns when they pass 15,000 tokens.

## Handoffs

Every leaf states, in its file:

- **Input**: what it needs and where it is (file path, or the fields the caller must pass).
- **Output**: exact file path and format, plus a short report back to the caller.
- **Done when**: a checkable condition.

Leaves pass work through files in a shared working folder, not through long messages. The caller passes paths.

## Naming

- Root slug: short, lowercase, no spaces (`xix`, `studio-ana`, `acme-logistics`).
- Agent names: `<root>-<branch>-<leaf>`, for example `xix-marketing-carousel-writer`. Leads: `<root>-<branch>-lead`.
- Names say the job, not a persona. `carousel-writer`, not `creative-genius`.
