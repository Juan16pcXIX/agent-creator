# Research

There are two research passes. Keep them apart.

## Pass 1: quick scan (phase 2, before the interview)

Goal: understand the root well enough to draw the tree. 3 to 5 searches total.

Search for:
- how this kind of project is usually organized (roles, departments, workflow)
- the common gaps or failure points for it
- anything specific the user mentioned that you do not know (a platform, a niche, a regulation)

Output: the five-line diagnosis from SKILL.md. Nothing goes to disk.

## Pass 2: deep research (phase 6, after approval)

Goal: give each agent the knowledge a real professional at the chosen level would bring. Depth depends on the expertise level:

| Level | Per branch | Per leaf | Sources |
|---|---|---|---|
| Practical | 1 or 2 searches | A 5-item checklist | Any reputable source |
| Professional | 3 to 5 searches | Checklist + common mistakes + the 1 or 2 frameworks pros use | Practitioner sources, official docs, recent (last 2 years when the field moves fast) |
| Expert | 5+ searches plus one focused pass per leaf | Checklist + mistakes + frameworks + benchmarks or numbers + how experts disagree | Primary sources, research, official data; every number linked |

### How to run it

For professional and expert levels, launch one research subagent per branch in parallel (Agent tool, `model: "sonnet"`). Each brief must stand on its own, because the subagent starts with no context:

```
You are researching the <BRANCH> area for <ROOT SUMMARY IN 2 LINES>.
The agents that will use this research:
- <leaf>: <objective>
- <leaf>: <objective>
Expertise level: <level>. That means: <row from the table above>.
Find: the frameworks and methods professionals use, a quality checklist for each
leaf, the most common mistakes, tools or sources that matter, and (expert only)
benchmarks with numbers.
Rules: every important claim has a link. Anything you could not verify is marked
[UNVERIFIED]. No filler, no generic advice that applies to any field.
Write the result to <ABSOLUTE PATH>/knowledge/<branch>.md using the structure below,
and reply with a 5-line summary.
```

For the practical level, do the research yourself; it is small enough.

If the user has a research skill installed (for example a deep-research or last-30-days skill), use it for areas where recent information matters, such as social media trends.

### Knowledge file structure

```markdown
# <Branch> knowledge for <root>

Level: <level> · Researched: <date>

## How professionals approach this
<frameworks and methods, short, with links>

## Per agent
### <leaf name>
- Quality checklist: ...
- Common mistakes: ...
- Benchmarks (expert): ...

## Tools and sources worth using
...

## Open questions / unverified
...
```

Keep each knowledge file under about 400 lines. Agents read it on every run, so length costs tokens every time.
