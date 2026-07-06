---
name: frontend-perf-auditor
description: Diagnose and fix front-end performance — bundle size, render cost, load waterfall — from measurement, not guesses. Use when a page is slow, the bundle is huge, or asked "why is this slow", "reduce bundle size", "optimize load time", "fix the jank". Measure first, fix the biggest thing.
---

# Frontend Perf Auditor

Perf work without measurement is superstition. Find the actual bottleneck, fix the one that
moves the number, verify it moved. Optimizing the wrong thing is wasted effort with new risk.

## Measure first (what's actually slow)

- **Load**: what's on the critical path? A blocking script, a giant JS bundle, an unoptimized
  hero image, a render-blocking font, a slow API call the page waits on. Read the network
  waterfall, not your intuition.
- **Bundle**: run the analyzer. What's biggest? Usually a heavy dependency (a whole date/chart/
  icon library for one function), duplicate deps, or no code-splitting.
- **Runtime/jank**: profile. Long tasks, excessive re-renders, layout thrash, unthrottled
  handlers, big lists rendered all at once.

## Highest-value fixes (apply the one the measurement points to)

- **Ship less JS**: code-split by route, lazy-load below-the-fold and heavy components,
  tree-shake, replace a heavy lib with a lighter one or a few lines. JS is the most expensive
  byte (parse + execute, not just download).
- **Images**: right format (AVIF/WebP), right size (don't ship 4K for a thumbnail), lazy-load
  offscreen, set dimensions to avoid layout shift.
- **Render**: memoize expensive components, virtualize long lists, avoid state that re-renders
  the world, debounce/throttle high-frequency handlers.
- **Network**: parallelize independent requests, cache, prefetch the likely-next, don't block
  first paint on non-critical data.

## Verify

Re-measure the same metric after the change. If it didn't move, revert — you added complexity
for nothing. Chase the biggest bottleneck, not a 2% win while a 2 MB bundle sits there.

## Rules

- No profile/measurement, no fix. Never optimize on a hunch.
- Fix the biggest bottleneck first; micro-optimizing around a huge one is theater.
- Re-measure to confirm; keep only changes that actually moved the metric.

## Output

The measured bottleneck (with the number), the specific fix, before→after on that metric, and
the trade-off if any (complexity, cache invalidation).
