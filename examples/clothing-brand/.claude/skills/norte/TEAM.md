# Norte: team map

Example output of agent-creator · Level: professional · Budget: balanced

## Tree

```
norte  (/norte)
├── marketing  ── norte-marketing-lead  [sonnet · medium]
│   ├── strategist        [opus · high]     2-4 week content and campaign plan
│   ├── trend-scout       [haiku · medium]  this week's trends, with links
│   ├── carousel-writer   [sonnet · medium] one carousel from a brief
│   ├── fact-checker      [sonnet · medium] checks claims, prices, dates
│   ├── canva-designer    [sonnet · medium] builds the carousel in Canva   [yours]
│   └── product-photo-editor  [inherits]    product photos for the store  [exists, kept]
└── finance  (no lead: only two leaves)
    ├── bookkeeper           [haiku · low]     categorizes transaction exports
    └── cashflow-forecaster  [opus · medium]   13-week forecast, risky weeks
```

## Agents

| Agent | Score | Model | Why |
|---|---|---|---|
| marketing-lead | 5 | sonnet · medium | Routes and sequences; strategy lives in the strategist (lead rule) |
| marketing-strategist | 9 | opus · high | Open brief, multi-week horizon, taste decides the result |
| marketing-trend-scout | 4 | haiku · medium | Lookups with a clear filter, checked later by the fact-checker |
| marketing-carousel-writer | 6 | sonnet · medium | Production writing from a brief; audience-facing floor |
| marketing-fact-checker | 5 | sonnet · medium | Verifier rule: as strong as the writer it checks |
| marketing-canva-designer | 4 | sonnet · medium | Added by the user during setup; works through the Canva connector; audience-facing floor |
| product-photo-editor | - | inherits | The user already had it and chose to keep it as is |
| finance-bookkeeper | 2 | haiku · low | Rule-based categorization, totals are checkable |
| finance-cashflow-forecaster | 8 | opus · medium | Money at stake, 13-week horizon, many dependent assumptions |

## Cost picture

- Haiku: 2 agents (the bookkeeper runs on every export; the scout weekly)
- Sonnet: 3 agents (the writer runs once per carousel: the most frequent call)
- Opus: 2 agents (the strategist once per period; the forecaster monthly)
- A typical week ("plan next week and write the 3 carousels") fires: lead, scout, strategist, writer x3, fact-checker x3.

## Example requests

- `/norte plan the content for the March drop`
- `/norte write the carousel for piece 04 of the plan`
- `/norte here is February's bank export: finance/raw/feb.csv`
- `/norte can we afford to order 300 hoodies for April?`
