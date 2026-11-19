# Model routing

Each agent runs on the cheapest model that still does its job well. Score the job, map the score to a tier, then adjust.

## Step 1: score five dimensions (0, 1 or 2 each)

| Dimension | 0 | 1 | 2 |
|---|---|---|---|
| **Reasoning depth** | Follows a recipe (rename, format, fetch, transcribe, fill a template) | Combines a few known patterns (write a post from a brief, fix a known bug, summarize sources) | Builds something new from first principles (strategy, architecture, diagnosis of an unclear problem) |
| **Ambiguity** | Input is complete and the output format is fixed | Some gaps the agent fills with sensible defaults | Open brief; the agent has to decide what the problem even is |
| **Cost of an error** | Easy to spot and redo, nobody outside sees it | Wastes time or money if wrong, caught by a later check | Reaches customers, money, health, legal or the public; or nothing downstream will catch it |
| **Horizon** | One output, one sitting | A small set of linked outputs (a week of posts, a feature) | A plan over weeks or months, or many dependent steps |
| **Judgment** | Right and wrong are obvious | Taste matters, but there are clear references to follow | Taste, trade-offs and priorities decide the result |

Total: 0 to 10.

## Step 2: map the score to a tier

| Score | Model | Effort | Typical jobs |
|---|---|---|---|
| 0 to 2 | `haiku` | `low` | File handling, downloading, renaming, formatting, transcribing, scheduling, filling templates, data entry, simple lookups |
| 3 to 4 | `haiku` | `medium` | Tagging, sorting, first-pass extraction, checklists against a clear rule set |
| 5 to 6 | `sonnet` | `medium` | Most production work: writing a carousel or post, editing copy, coding a component, summarizing research, drafting emails |
| 7 | `sonnet` | `high` | Research with sources, writing that needs a strong voice, debugging, fact-checking |
| 8 | `opus` | `medium` | Multi-week plans, content calendars, campaign strategy, architecture, final QA on high-stakes output |
| 9 to 10 | `opus` | `high` | Business strategy, pricing, legal or financial reasoning, the decision-maker of a branch |

## Step 3: adjust

Apply in this order, and write down which adjustments you used (it goes in the "why" column the user sees).

1. **Budget** (from the interview)
   - Economical: subtract 1 from every score, and cap at `opus` effort `medium`.
   - Balanced: no change.
   - Max quality: add 1 to every score of 7 or more. Scores that reach 10 may use `fable` instead of `opus` (most capable, most expensive; only when the user picked max quality).
2. **Volume.** If an agent will run many times per request (one call per file, per post, per product), drop it one tier unless its cost of error is 2. Volume multiplies cost.
3. **Verifiers.** A verifier must be at least as strong as the agent it checks, and never `haiku` when the cost of error is 2.
4. **Leads.** A lead that only routes and sequences: `sonnet` `medium`. A lead that sets strategy or makes the final call for its branch: `opus`.
5. **Floor.** Anything that writes text a customer or audience will read is at least `sonnet`.

## Relative cost (why this matters)

API list prices per million tokens, input / output, as of November 2026. Check current prices before quoting them.

| Model | Input | Output | Relative to Sonnet |
|---|---|---|---|
| Haiku 5.5 | $0.10 | $0.50 | about 1/20 |
| Sonnet 5.5 | $2 | $10 | 1 |
| Opus 5.5 | $4 | $20 | 2x |
| Fable 5.2 | $8 | $40 | 4x |

On a Claude subscription the same ratios show up as how fast you burn through your usage limits. A team where the mechanical work runs on Haiku and only the strategy runs on Opus does the same job for a fraction of an all-Opus team.

## Allowed values in agent frontmatter

- `model`: `haiku`, `sonnet`, `opus`, `fable`, `inherit`, or a full model id. Prefer the aliases: they move to the newest model of each tier automatically.
- `effort`: `low`, `medium`, `high`, `xhigh`, `max`. Available levels depend on the model. Use `xhigh` or `max` only when the user explicitly asks.

## Worked examples (clothing brand)

| Agent | Depth | Ambig. | Error | Horizon | Judgment | Score | Result |
|---|---|---|---|---|---|---|---|
| Image resizer for product photos | 0 | 0 | 0 | 0 | 0 | 0 | haiku / low |
| Trending topics scout | 1 | 1 | 1 | 0 | 1 | 4 | haiku / medium |
| Carousel writer | 1 | 1 | 1 | 1 | 2 | 6 | sonnet / medium |
| Fact-checker for marketing claims | 1 | 1 | 2 | 0 | 1 | 5 -> verifier rule | sonnet / medium |
| Two-week content strategist | 2 | 2 | 1 | 2 | 2 | 9 | opus / high |
| Cash-flow forecaster | 2 | 1 | 2 | 2 | 1 | 8 | opus / medium |
