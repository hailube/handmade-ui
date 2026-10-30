---
name: designing-for-real-content
description: Use when building or reviewing any list, grid, feed, card, table or detail view that renders data from an API — especially when the design was built against short placeholder strings, when loading/empty/error states are missing, when images come from users, or when the real collection is far larger than the mock. Symptoms include layout shift, blank cards, clipped names, a spinner for the whole page, and lazy loading that never fires.
version: 1.0.0
license: MIT
---

# Designing for Real Content

A design built against `["Chess Club", "Robotics"]` meets `["Northeastern University Competitive Programming and Algorithmic Problem Solving Society", ""]` and breaks.

Core principle: **the mock is the best case. Design the worst case, because that is what users see.**

## The four states are not optional

Every data-backed view has four, and generated UI reliably ships one or two.

| State | What it must do | Usual failure |
|---|---|---|
| **Loading** | Hold the layout it is about to fill | A centred spinner replacing the page, so everything jumps when data lands |
| **Empty** | Say why it's empty and offer the action that fills it | "No results." — dead end |
| **Error** | Say what failed and how to retry, in the interface's voice | Nothing; the view renders as empty, so a 500 looks like zero clubs |
| **Loaded** | The design | — |

Empty-because-new and empty-because-filtered are different screens: one invites creating, the other invites clearing a filter.

Skeletons beat spinners when you know the shape, because they reserve the space. A skeleton that doesn't match the real layout's dimensions is worse than a spinner — it shifts twice.

## Content extremes to design against

Build the component, then feed it these before you call it done:

- **Longest realistic string**, not "Chess Club". Will it wrap, clamp or clip? If it clamps, is the clamped form still identifying? Two clubs whose names differ at word 9 both render as "Northeastern University Competitive…".
- **Empty or missing** every optional field. A card with no logo, no category, no description.
- **One item, and the real count.** A grid of 3 and a grid of 600 are different designs. So are 1 and 0.
- **The ugliest real image.** User-supplied logos arrive as 3000px PNGs with transparent backgrounds, wrong aspect ratios, and white-on-white. `object-fit` is a decision: `cover` crops logos badly, `contain` leaves letterboxing you must style.
- **A failed image.** Always have the fallback — initials, monogram, or a shape. A broken-image icon is a visible defect.
- **Slow.** Throttle to 3G. Most perceived-quality failures only appear here.

## Images and lists at real scale

Three failures worth knowing because they are invisible on a mock:

**`loading="lazy"` is measured against the viewport, not your scroller.** If the list scrolls inside a container rather than the page, the look-ahead margin does not carry through, so images are requested only once already visible — reading as blank cards that paint in strips. Inside a nested scroller you need an `IntersectionObserver` rooted on that container. Sideways rows need the same along the x-axis.

**One observer per container, not per element.** 600 cards means 600 observers, which costs more than the images.

**`content-visibility: auto` skips off-screen layout — and that interacts with observers.** An element with no layout never intersects, so observe the *card*, not the image inside it.

For very large collections, virtualise. Before reaching for a library, check whether `content-visibility: auto` with `contain-intrinsic-size` is enough; it often is, and costs two lines.

## Layout that survives content

- Size columns by content, not viewport: `repeat(auto-fill, minmax(min(100%, 14rem), 1fr))`. The `min(100%, …)` stops overflow at 320px. Viewport breakpoints are a second resort.
- Anything that can receive a long unbroken string needs `min-width: 0` on the flex/grid child, or it refuses to shrink and overflows its parent.
- Reserve image space with `aspect-ratio` so loading doesn't reflow.
- Interactive cards should be one `<button>` or `<a>`, not a `<div>` with `onClick`. You get keyboard reach and focus for free.
- Gate hover affordances to `(hover: hover) and (pointer: fine)` — touch devices fire a hover on tap, so the lift sticks after the finger leaves.

## Quality floor

Non-negotiable, and cheap if done while building rather than after: visible keyboard focus on every interactive element; accessible names on icon-only controls; reduced motion respected; usable at 320–400px wide; text contrast ≥ 4.5:1 (check the colour against the surface it actually sits on, not against white).

## Verify by looking

This skill cannot be satisfied by reading code. Render it, then:

1. Load with the network throttled and watch the first second. That is where perceived quality lives.
2. Resize to 400px.
3. Tab through it.
4. Replace the data with the extremes above.

If your environment has a browser tool, take screenshots and look at them. "I have not rendered this" means the work is not finished.

## Common mistakes

- **Shipping the loaded state and calling it done.** Three of four states missing is the most common generated-UI defect.
- **Treating error as empty.** Silently rendering an empty list on a failed fetch tells the user there is nothing there.
- **Testing with seeded data that is all the same length.** Uniform mocks hide every wrapping bug.
- **A fixed card height to "keep it tidy".** It clips the long names instead of showing them.
