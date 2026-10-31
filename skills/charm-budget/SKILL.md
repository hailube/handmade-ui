---
name: charm-budget
description: Use when a UI has personality to spend — a landing page, a marketing surface, a product with a distinct voice — or when reviewing one that feels busy, uniformly decorated, or like several designs at once. Symptoms include a hover lift on every card, gradients and shadows applied everywhere, three accent colours, four border-radii, decoration that survived a redesign because nobody knew it was optional.
version: 1.0.0
license: MIT
---

# Charm Budget

Decoration spent everywhere reads as decoration spent nowhere. The eye needs somewhere to land, and it can only land if the surroundings are quiet.

Core principle: **write down where charm is spent *and where it is refused*, before writing the CSS.** Restraint that exists only as taste erodes on the next edit; restraint written as a contract survives.

## The artifact

A short paragraph at the top of the stylesheet or kit. Two halves — the spend list and the denial list. The denial list is the half that does the work.

```css
/* The kit every prototype is built from, so they differ in layout and in how you
   find a trail, never in decoration.

   Topographic, with the map's charm: a contour-line header fading into the page,
   elevation profiles drawn as hand-inked strokes, trail names in condensed caps,
   and the map's press-down buttons.
   Charm is spent in those places and nowhere else: no tilted-at-rest cards, paper
   grain, compass roses or textures beyond the contour header itself. */
```

Why the denial list matters: without it, the next contributor adds a tilt to a card because tilting is *on-brand*, and they are right — it is. The denial list is the only thing that distinguishes "we didn't think of it" from "we decided against it." It converts taste into a reviewable rule.

## Writing one

1. **Name the one memorable thing.** If everything is memorable, nothing is. One element may be bold; everything else supports it.
2. **List where charm is spent** — three to six specific places, each naming the device and its location. "Elevation profiles as hand-inked strokes" is a budget line. "Playful and warm" is not.
3. **List what is refused**, especially the on-brand things you are *not* doing. These are the ones that will otherwise creep back.
4. **Say what the plain parts are.** "Charm is spent in those places and nowhere else" is the enforcing clause.

## Where the budget usually leaks

Each of these is decoration applied by default rather than by decision:

| Leak | Why it happens | The budget question |
|---|---|---|
| Hover lift + shadow on every card | It's the default card treatment | Does hovering a card need to say anything beyond "clickable"? |
| All-caps tracked labels above content | Reads as "designed" | Does this label carry information the heading doesn't? |
| Gradient as background on unrelated surfaces | Fills empty space | Is this gradient the memorable thing, or filler? |
| Three accents | Each added for one component | Which single accent is the product's? |
| Four border-radii | Each picked locally | What is the radius *system* — one value, or one plus a deliberate exception? |
| An icon beside every heading | Symmetry | Does the icon distinguish anything? |
| Shadow on everything at one elevation | `box-shadow` copied between rules | Does shadow encode depth here, or is it texture? |

A device used everywhere stops encoding anything. Structural devices — borders, numbering, dividers, eyebrows, labels — should carry information. Numbered markers are right for a sequence and wrong for a list of features.

## Exploring before committing

When the direction isn't settled, build a few and compare rather than arguing in the abstract. The discipline that makes the comparison mean anything:

- **One axis varies.** Pick what you're actually choosing between — structure, density, emphasis, or type — and let secondary choices follow from it. A variant that changes the accent colour and the copy too is not a comparison.
- **Hold the copy constant.** Then the comparison is of design, not of writing. Write it once, use it in all of them.
- **Real content and the real count.** A direction that works at 6 items and dies at 600 should die now.
- **Same quality floor in each.** Keyboard reach, focus, reduced motion, 400px. A direction that can't meet the floor isn't a direction.
- **Name them by direction** — Contour, Ledger, Postcard — not Option A. The name is the thesis.
- **Each behind its own route or URL parameter**, so every one is a shareable link and nothing is wired into production yet.

Then pick, promote the winner through one indirection so the move is a one-line change, and **delete the others**:

```js
/* Where the landing page lives. Only the /landing previews today; point LANDING_PATH
   at '/' and add '/' to isLandingPath when the chosen design moves to the root. */
export const LANDING_PATH = '/landing/contour';
```

**Leave a signpost.** Unreferenced preview routes are invisible — no nav links to them, so nobody finds them and everybody assumes the work didn't land. Put the URLs in the PR body and the README. This is the single most common way good design work goes unseen.

## Review pass

- Read the budget, then the screen. Anything decorated that isn't on the spend list: remove it or amend the budget deliberately.
- Count the accents, the radii, the shadow elevations, the typefaces. Each count above one needs a reason.
- Chanel's rule: before shipping, remove one accessory. If the design is worse, put it back; usually it isn't.

## Common mistakes

- **A budget of adjectives.** "Warm, playful, modern" constrains nothing and permits everything.
- **Spend list without denial list.** Half the artifact, and the half that doesn't hold.
- **Writing it after the CSS.** Then it describes what you built instead of governing it.
- **Confusing restraint with plainness.** The budget exists so the bold thing can be bolder, not so nothing is bold.
