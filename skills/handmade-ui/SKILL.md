---
name: handmade-ui
description: Use when building, reviewing or polishing UI and the result needs to read as made by a person — a screen that looks templated or AI-generated, a design that falls apart on real data, decoration applied uniformly, or design values with no stated reason. Also use when deciding which design skill a UI task needs.
version: 1.0.0
license: MIT
---

# Handmade UI

Generated UI reads as generated because **nothing in it was caused by anything**. A human designer's work is full of values that could only have come from this product: a colour darkened because it sits on a photographic scrim, a duration that ends when a data fetch begins, a label that exists because users kept asking one question.

Core principle: **a design choice that cannot name its cause is a default wearing a costume.**

## Pick the layer

This collection owns the craft layer. For the layers above and below it, use the skill that already covers them — do not reimplement them here.

| Task | Skill |
|---|---|
| Aesthetic direction, typography, palette, anti-default calibration, copy | `frontend-design` (Anthropic) |
| Motion: curves, durations, springs, gestures, interruption, reduced motion | `animate` / `review-animations` (emilkowalski) |
| Exploring several directions before committing | A variant-exploration skill, or the method in `charm-budget` → Exploring |
| Charts, dashboards, stat tiles | `dataviz` |
| **Every design value carrying its reason** | **`design-provenance`** |
| **Real content lengths, counts, missing, slow, failed** | **`designing-for-real-content`** |
| **Where decoration is spent and where it is refused** | **`charm-budget`** |

## Order

1. Direction first (`frontend-design`). You cannot justify values against a brief you don't have.
2. Write the budget (`charm-budget`) before the CSS. It is cheap to write and expensive to retrofit.
3. Build with provenance (`design-provenance`). Reasons written as you go, not archaeology afterwards.
4. Feed it real content (`designing-for-real-content`) before calling it done.
5. Look at it in a browser at 400px wide. Nothing in this collection substitutes for looking.

## Why written reasons, specifically

Measured, same model, same task, two contexts: asked for a card grid in a codebase whose design decisions were written down in comments, it reproduced the codebase's hard-won solutions — including a lazy-loading fix for nested scrollers it could not have derived. Asked for the same grid in an empty directory, it shipped `loading="lazy"` (defeated by a nested scroller), all-caps tracked labels, a hover lift on every card, four unrelated border-radii, and a comment density of 0.6% against the good codebase's 27%.

The difference was not capability. It was whether the reasoning had been written where it could be read. Write design reasons down and they propagate; leave them in your head and the next editor — human or agent — reaches for the default.
