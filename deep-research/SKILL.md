---
name: deep-research
description: Run a rigorous, multi-source research investigation and produce a cited, fact-checked report. Use for any question that needs depth and verification — market/competitor/technical research, "research X thoroughly", "give me a report on Y", "what's the state of Z". Fans out, verifies claims adversarially, synthesizes with citations. A comprehensive skill.
---

# Deep Research

Shallow research confidently repeats whatever ranks first. Deep research triangulates,
verifies, and tells you how sure it is. The output is a report you could act on, with the
receipts and the caveats.

## Phase 0 — Scope (don't skip)

- Pin the exact question. If it's vague ("is X good?"), narrow it: good for whom, versus what,
  on which criteria, as of when. Ask 1-2 clarifying questions if the scope changes the answer.
- Decide what a good answer looks like (the criteria/dimensions) before searching — so you
  research against a frame, not vibes.

## Phase 1 — Fan out (breadth first)

- Search several angles, not one query reworded: by entity, by claim, by counter-claim, by
  time ("X 2026"), by source type (primary docs, data, expert commentary, dissent).
- Prefer PRIMARY sources: the vendor's own doc/pricing, the paper, the filing, the dataset —
  over aggregators and listicles that summarize (and distort) them.
- Cast wide enough to find disagreement. If every source agrees, you probably haven't found
  the critique yet — keep looking for it.

## Phase 2 — Verify (the part that makes it "deep")

For every load-bearing claim (a number, a "best", a causal statement):
- **Trace to the primary source.** A figure repeated by ten blogs traced to zero primary
  sources is a rumor with good PR. Find where it actually originated — or drop it.
- **Adversarially check it**: actively look for the source that contradicts it. Steel-man the
  other side. A claim that survives a genuine attempt to refute it is worth keeping.
- **Watch the traps**: stale data presented as current; a vendor's self-report treated as
  independent; correlation dressed as causation; a benchmark measured differently across sources
  (don't compare them); a negative claim ("X has no Y") that's just missing from your sources, not false.
- Rate confidence per claim: confirmed (multiple independent/primary) · likely · single-source · disputed.

## Phase 3 — Synthesize

- Answer the actual question first, plainly (the "if you read one line" version).
- Then the structured findings by dimension, each with its evidence and confidence.
- **Show the disagreement** where sources conflict — don't average it away into a false consensus.
- State what you could NOT verify and what would change the conclusion. Uncertainty named is
  trustworthy; uncertainty hidden is a liability.
- **Cite every load-bearing claim** to its source. A report you can't check isn't research, it's an opinion.

## Rules

- Primary sources over summaries; a claim's value is its traceability, not its repetition count.
- Verify before you include; adversarially, not just confirmingly. Confirmation bias is the whole failure mode.
- Report confidence and dissent honestly. "I'm not sure, here's why" beats a confident wrong answer.
- Never invent a citation. If you can't source it, say so or cut it.

## Output

```
RESEARCH — <question>
Answer (1 paragraph): <the direct answer, plainly>
Findings by dimension:
  <dimension> — <finding> [confidence] — sources: <cited>
Disagreements: <where sources conflict + your read>
Couldn't verify: <open questions>
What would change this: <the decisive unknowns>
Sources: <list — primary flagged>
```
