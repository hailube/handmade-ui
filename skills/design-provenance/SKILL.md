---
name: design-provenance
description: Use when writing or reviewing CSS, design tokens, durations, spacing, colour values or layout constants — especially when values are round numbers, a palette looks borrowed, comments restate what the code does instead of why, or a reviewer cannot tell which choices were deliberate. Also use when design decisions keep getting undone by later edits.
version: 1.0.0
license: MIT
---

# Design Provenance

Every design value carries its cause, written where the next editor will read it.

Core principle: **state the cause at the value, not in your head.** A value whose reason lives only in the author's memory gets reverted by the next person who finds it arbitrary — and to them it is arbitrary.

## The test for any value

Ask: *what would have to be different about this product for this number to change?*

- No answer → it's a default. Either derive it or admit it in a comment.
- Answer is a general principle ("modals should feel snappy") → weak. Principles produce the same value for every product.
- Answer names something specific to this product ("it sits on the photo scrim, and a muted grey disappears there") → that's provenance.

## Round numbers are a smell

`300ms`, `16px`, `#333`, `0.5`, `100vw` are where defaults hide. They are not wrong — a round number that was *checked* is fine. But an unexamined round number is the single most reliable tell of generated UI.

Prefer values that are **derived** over values that are **picked**:

```js
// ✅ Derived: each panel travels exactly far enough to clear its own screen edge.
const travel = side < 0 ? -((index + 1) * share + GAP) : (PANELS - index) * share + GAP;

// ❌ Picked: why 100? what breaks at 90?
const travel = side < 0 ? -100 : 100;
```

When a value must be picked, say what it was picked against:

```css
/* Darker than a muted grey would be: it sits on the photo scrim, and a lighter
   grey disappears there. */
--ink-muted: #6a5f52;
```

## Comment the why, never the what

The code already says what it does. Reviewers skip comments that restate it, so a file of *what* comments reads as a file with no comments.

| Form | Example | Verdict |
|---|---|---|
| Restates the code | `/* The grid scrolls inside this box. */` | Useless — `overflow-y: auto` said that |
| Names a principle | `/* Snappy, for a better feel. */` | Weak — true of every product |
| **Names the observed symptom** | `/* Native lazy loading gets no look-ahead margin inside a nested scroller — images painted in strips from the top down as you scrolled. */` | Provenance |
| **Names the constraint** | `/* A fixed tween, not a spring: the panel starts its fetch in onAnimationComplete, so it must end at a known time. A spring's settle varies with whatever interrupted it. */` | Provenance |

The best form is **symptom → cause → fix**, because it tells the next editor what will come back if they undo it.

## Tokens encode relationships, not just values

A token that only names a colour is a variable. A token that other components measure themselves against is structure — and it must say so, or someone will "simplify" it.

```css
/* The header's full height, sticky search included: the split view and the detail
   pane size their panes and sticky offsets from it. */
--header-h: 90px;
```

## Single-source anything that can drift

Two places holding the same number will diverge. When JS timing and CSS timing must agree, one owns it and passes it to the other:

```js
const CLOSE_DUR = 620;
// Passed to the CSS as variables so the stylesheet and the timers can't drift apart.
const CSS_TIMING = { '--close-dur': `${CLOSE_DUR}ms` };
```

Scope tokens to the thing they belong to, not `:root`, when they shouldn't escape it:

```css
/* Scoped to .landing rather than :root so nothing here leaks into the app
   once a visitor clicks through and signs in. */
.landing { --paper: #e8d2b0; }
```

## Review pass

Read your own diff and, for each value you introduced, answer the test above. Then:

- Values you can't justify: delete, derive, or comment as deliberate-and-unchecked.
- Comments that restate code: replace with the symptom, or delete.
- The same number in two files: give it one owner.
- A value you tuned by eye: say so, and say what to look at. `/* 1.3 overshoot: check at 3x that it doesn't read as a wobble at full-viewport size. */` is honest and useful; silence is not.

## Common mistakes

- **Commenting everything.** Provenance is for values that were *decided*. `display: flex` needs no comment. Target the numbers and colours.
- **Writing the reason in the PR instead of the file.** PRs are unreadable six months later and invisible to the next agent. The file is the only durable place.
- **Justifying after the fact.** Reasons invented at review time are rationalisations and sound like it. Write them as you choose.
- **Treating a library default as derived.** `theme.spacing(2)` is a default unless you chose that scale.
