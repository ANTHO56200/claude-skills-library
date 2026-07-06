---
name: component-architecture
description: Design or refactor a UI component with clean props, state, and composition boundaries. Use when building a non-trivial component, when one has grown messy, or when asked "how should I structure this component", "this component is too big", "review my component design". Right-sizes responsibilities.
---

# Component Architecture

A component should do one thing, own the minimum state, and be obvious to reuse. Most
component pain is a boundary drawn in the wrong place — fix the boundary.

## Decide the boundaries

- **One responsibility.** If you can't name a component in a short phrase, it's doing too
  much — split it. A 400-line component is usually 4 components and a hook.
- **Presentational vs container**: separate "how it looks" (props in, events out, no data
  fetching) from "where data comes from". Presentational components are trivially testable
  and reusable; keep them dumb.
- **Composition over configuration**: a component with 15 boolean props that fork rendering
  should be several components (or use `children`/slots). Flags that change layout = split.

## State: own the least, lift only when shared

- Local UI state (open/hover/input) → in the component.
- State two siblings need → lift to the nearest common parent, no higher.
- Server data → a data hook / query layer, not tangled into render.
- **Derive, don't store**: if a value can be computed from props/state, compute it — don't
  duplicate it into state that can drift out of sync.

## Props: a clean contract

- Minimal and specific. Pass data, not the whole object if only a field is used.
- Events out via callbacks (`onSelect(id)`), not by reaching into the child.
- Sensible defaults; required vs optional explicit. Avoid boolean-prop explosion (see composition).
- Don't leak internal structure through props — the parent shouldn't need to know how the child works.

## Rules

- Draw the boundary by responsibility and reuse, not by line count alone — but line count is a smell.
- Prefer composition and derived state; they remove whole classes of bugs.
- A component that fetches, transforms, styles, and handles 6 events is 4 jobs — separate them.

## Output

The component structure (which pieces, what each owns), the prop/event contract for each,
where state lives, and the specific splits/extractions if refactoring — with the reason for each.
