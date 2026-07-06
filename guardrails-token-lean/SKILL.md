---
name: guardrails-token-lean
description: Estimate the token cost of an approach before committing to it, size it to what the task needs, and stop once extra spend stops paying off. Use when the user says "keep token usage down", "this is burning tokens", "what's the cheapest way to do this", "token budget", "don't blow the budget on this" — or before any agent fan-out, large file/DB dump into context, or open-ended retry loop.
---

# Guardrails (Token-Lean)

Token spend is a decision made before the first call, not a cleanup done after. Estimate
the bill an approach will run, size it to what the task actually needs, and stop once extra
spend stops buying anything new.

## Estimate before you commit

- Fan-out (N agents, N parallel searches, N file reads) costs roughly N × cost-per-unit.
  Do that multiplication before launching it, not after the bill is in.
- About to pull a big file, a full DB table, or a long log into context? Estimate its size
  first (line count, row count, rough chars ÷ 4) — that decides whole-read vs. narrow-read.
- About to retry something that might fail again? Estimate retries × cost-per-attempt and
  set the cap before the first attempt, not after the third failure.

## Size the approach to the task

- Match model tier to the sub-task: cheap/fast tier for filtered extraction, formatting, or
  well-specified mechanical work; top tier reserved for the step that actually needs the
  reasoning. Don't default every step to the most expensive tier.
- Read narrow: offset/limit on the lines you need, grep for the exact symbol/string, a
  filtered query for the rows that matter — not the whole file, the whole tree, or `select *`.
- Batch what can be batched. One query for N rows beats N queries; one message with N
  independent calls beats N round-trips.

## Set the cap up front

- Fan-out ceiling — e.g. "more than 20 files → sample a representative handful and report
  the rest by pattern," not process all 20 individually.
- Retry ceiling — a fixed max (e.g. 3 attempts), then stop and report, not open-ended retry.
- Read ceiling — a rough total-lines/tokens budget for exploration before switching from
  "look around" to "read exactly this."

## Stop when returns flatten

- Each extra agent, search, or retry should still surface something new. When the last
  couple came back empty or just repeated what you already had, the next one will too —
  stop there.
- A restated result is not a second data point.
- "Good enough to answer the question" beats "exhaustive" unless exhaustive was the ask.

## Output

State the planned spend before non-trivial work (roughly how many agents/reads/calls, at
what tier). When reporting back, state what was actually spent against that plan — and why,
if it diverged.

## The rule

Every fan-out, every full-file read, every retry loop is a bill picked before it's spent.
Estimate it, cap it, stop once the returns flatten.
