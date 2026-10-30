# Handmade UI

Claude Code skills for the layer of UI work between *picking a look* and *surviving real content*.

Generated UI reads as generated because **nothing in it was caused by anything**. These three skills attack that directly: they make Claude justify the values it writes, design for the data it will actually receive, and spend decoration on purpose instead of uniformly.

## The skills

| Skill | Use when |
|---|---|
| **`handmade-ui`** | Router. Deciding which design skill a UI task needs. |
| **`design-provenance`** | Writing or reviewing CSS, tokens, durations, spacing, colour. Values are round numbers, the palette looks borrowed, comments restate the code. |
| **`designing-for-real-content`** | Any list, grid, feed, card or table backed by an API. Missing loading/error states, layout shift, clipped names, lazy loading that never fires. |
| **`charm-budget`** | A surface with personality to spend, or one that feels busy and uniformly decorated. Hover lift on every card, three accents, four radii. |

## What this is *not*

These skills deliberately **do not** cover ground that is already well covered. Use them alongside:

- [`frontend-design`](https://github.com/anthropics/claude-code) (Anthropic) — aesthetic direction, typography, palette, anti-default calibration, copywriting. **Start here**; provenance needs a brief to justify values against.
- [`animate` / `review-animations`](https://github.com/emilkowalski/skills) (Emil Kowalski) — motion craft: curves, durations, springs, gestures, interruption, reduced motion. Comprehensive; don't reimplement it.
- A variant-exploration skill — building several directions and comparing them. `charm-budget` includes the comparison discipline but not the tooling.
- `dataviz` — charts, dashboards, stat tiles.

## Why written reasons

The design came out of one measurement. The same model was given the same task — "a scrollable grid of club cards with logo, name and category" — in two contexts.

**In a codebase whose design decisions were written down in comments**, it reproduced that codebase's hard-won solutions, including an `IntersectionObserver` lazy-loading fix for nested scrollers that it could not have derived from the task, and it explained *why* a fixed tween beat a spring for a panel whose data fetch starts when the animation ends.

**In an empty directory**, the same model shipped:

| | Empty directory | Codebase with written reasons |
|---|---|---|
| Comment density | **0.6%** | **27%** |
| Nested-scroller images | `loading="lazy"` — silently defeated | `IntersectionObserver` rooted on the scroller |
| Labels | `text-transform: uppercase` + tracking | — |
| Hover | lift + shadow on every card, not gated to fine pointers | gated to `(hover: hover) and (pointer: fine)` |
| Border radii | four unrelated values | one system |
| States | empty only | loading, empty, error, loaded |
| Verified in a browser | "has not been rendered" | — |

The difference was not capability. It was whether the reasoning had been written where it could be read. That is the whole thesis: **write design reasons into the artifact and they propagate; leave them in your head and the next editor — human or agent — reaches for the default.**

Motion, by contrast, came out *well* in the empty directory, because good motion skills already exist and were loaded. That is the evidence these skills sit in the right place.

## Install

### As a plugin (recommended for teams)

```bash
claude plugin marketplace add <your-org>/handmade-ui
claude plugin install handmade-ui
```

### As personal global skills

```bash
git clone https://github.com/<your-org>/handmade-ui.git
cp -R handmade-ui/skills/* ~/.claude/skills/
```

They are then available in every project. Restart Claude Code if they don't appear, and check each directory contains a file named exactly `SKILL.md`.

### In a single project

```bash
cp -R handmade-ui/skills/* .claude/skills/
```

## Using them

They trigger on their own from the descriptions, or invoke by name:

```
/design-provenance            review this stylesheet
/designing-for-real-content   this grid needs to handle 600 clubs
/charm-budget                 the landing page feels busy
/handmade-ui                  which design skill does this need?
```

Suggested order on new UI: direction (`frontend-design`) → budget (`charm-budget`) → build (`design-provenance`) → real content (`designing-for-real-content`) → look at it in a browser at 400px.

## Contributing

Each skill was written against a measured baseline: a scenario run without the skill, with the failures recorded, and the guidance written to address those specific failures. If you extend one, keep that discipline — run the scenario first, record what actually goes wrong, then write to it. Guidance written against imagined failures reads like it.

## Licence

MIT. See [LICENSE](LICENSE).
