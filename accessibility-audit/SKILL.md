---
name: accessibility-audit
description: Audit a UI/component for accessibility (WCAG) with concrete, checkable issues and fixes. Use when building or reviewing UI, before shipping a page, or when asked "check accessibility", "is this a11y compliant", "audit for screen readers". Findings map to WCAG and come with the fix.
---

# Accessibility Audit (WCAG)

Accessibility is not optional polish — it's whether a chunk of your users can use the
thing at all (and, increasingly, a legal requirement). Audit against real barriers, fix concretely.

## Check (grouped by impact)

### Blocking (a real user is locked out)
- **Keyboard**: every interactive element reachable and operable by keyboard alone; visible
  focus indicator; no keyboard traps; logical tab order. (Try it: unplug the mouse mentally.)
- **Names**: every button/link/input has an accessible name (visible label, `aria-label`, or
  associated `<label>`). Icon-only buttons WITHOUT a name are invisible to screen readers.
- **Images**: meaningful `<img>` has descriptive `alt`; decorative images have `alt=""`.
- **Forms**: inputs have labels (not just placeholders — placeholder is not a label); errors
  are announced and tied to the field (`aria-describedby`), not just colored red.

### Serious
- **Contrast**: text ≥ 4.5:1 (≥ 3:1 for large text); don't rely on color alone to convey
  meaning (add icon/text). Check the actual computed colors, states included.
- **Semantics**: use real elements — `<button>` for actions, `<a>` for navigation, headings
  in order, lists for lists. A `<div onClick>` is not a button.
- **Live regions / dynamic content**: updates (toasts, async results) announced via
  `aria-live`; modals trap focus and restore it on close.
- **Zoom/reflow**: usable at 200% zoom; no horizontal scroll from a fixed layout.

### Minor (friction, not a barrier — the task still completes)
- **Near-miss contrast**: slightly under the ratio on decorative or non-essential text
  (body text under the ratio stays Serious).
- **Faint focus ring**: a focus indicator that exists but is hard to spot — a missing one
  is Blocking.

## Output

```
A11Y AUDIT — <component/page>
BLOCKING / SERIOUS / MINOR
  [level] <issue> — <element> — WCAG <criterion> — Fix: <concrete change>
Passes: <what's already accessible>
```

## Rules

- Map each finding to a WCAG criterion and a concrete fix — "improve accessibility" helps no one.
- Prefer native semantics over ARIA. The first rule of ARIA: don't use ARIA if a native
  element does the job. Bad ARIA is worse than none.
- Test the keyboard path yourself in your head (tab, enter, escape) — it catches the most, fastest.
