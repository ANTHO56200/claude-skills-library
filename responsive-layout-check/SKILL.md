---
name: responsive-layout-check
description: Find responsive/layout issues across screen sizes before they ship. Use when building a page/component, on a "it breaks on mobile" report, or when asked "check responsive", "does this work on mobile", "fix the layout on small screens". Catches the common breakpoint failures.
---

# Responsive Layout Check

Most "it breaks on mobile" bugs are the same handful of mistakes. Check them across the
real breakpoints, fix at the source (the layout system), not with one-off hacks.

## Check across widths (~360, 768, 1024, 1440)

- **Horizontal overflow**: the #1 offender. A fixed width, a long unbroken string, a wide
  table/image, or a negative margin pushes the page wider than the viewport → horizontal
  scroll. Find it (an element wider than its container) and fix it (`max-width:100%`,
  `min-width:0` on flex children, `overflow-x:auto` on the wide thing, `word-break` on strings).
- **Touch targets**: interactive elements ≥ ~44px on touch; not crammed edge-to-edge.
- **Text**: readable size on mobile (don't scale below ~14-16px body); line length not absurd
  on wide screens (cap with `max-width` / `ch`).
- **Images/media**: `max-width:100%`, height auto; no fixed pixel dimensions that overflow.
- **Layout shift**: does content jump between breakpoints? Grid/flex reflow that hides or
  reorders critical elements?
- **Fixed/absolute elements**: headers, modals, FABs — do they cover content or overflow on small screens?

## Fix at the system level

- Prefer fluid units (%, `fr`, `rem`, `clamp()`, `min()`/`max()`) over fixed pixels.
- Flexbox/grid that wraps, not fixed columns. `min-width:0` on flex/grid children to let
  them shrink (the classic overflow cause).
- Mobile-first: base styles for small, layer up with `min-width` media queries.
- One scroll container per wide element (table, code block) — the page body must never scroll sideways.

## Output

Per breakpoint: issues found (element + symptom), the root-cause fix (not a band-aid), and
which are blocking (unusable) vs cosmetic.

## Rules

- Reproduce the actual overflow/break before fixing — guess-fixing responsive bugs adds hacks.
- Fix the layout rule, not the symptom. A `overflow:hidden` that clips content is a cover-up.
