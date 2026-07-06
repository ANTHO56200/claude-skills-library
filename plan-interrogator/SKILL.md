---
name: plan-interrogator
description: Stress-test a plan or spec BEFORE any code is written. Use when the user shares a plan, feature idea, or spec and wants it challenged ("grill my plan", "poke holes", "review this spec"), or before implementing any non-trivial feature. Runs a BOUNDED interrogation (max 3 rounds) and exits with a build-ready spec.
---

# Plan Interrogator

Interrogate a plan until it is safe to build — then STOP. The output is a spec the
implementer can follow without guessing. This is not conversation; every question
must change what gets built.

## Hard bounds (non-negotiable)

- **Max 3 rounds** of questions, **max 5 questions per round**.
- Never ask something the plan (or an earlier answer) already covers.
- Every question must name **what it blocks**: "Blocks: error handling in the import loop."
- If the user answers "you decide", make the call, record it in the spec under
  **Decisions made for you**, and move on. Do not re-ask.
- Exit EARLY the moment the completeness gate passes. Fewer questions is better.

## Question priority (spend your budget in this order)

1. **Undefined behavior** — inputs, states, or user actions the plan never mentions.
   "What happens when X is empty / duplicated / mid-flight?"
2. **Data shape and ownership** — exact fields, types, who writes, who reads,
   what is the source of truth when two things disagree.
3. **Failure modes** — what may fail (network, validation, permissions), what the
   user sees, whether the operation retries, rolls back, or half-completes.
4. **Scope boundary** — what is explicitly OUT. Unstated scope is how projects rot.
5. **Verification** — how a reviewer will confirm it works. If it cannot be
   verified, it is not a requirement; rewrite or cut it.

Skip any category the plan already answers. Do not invent hypotheticals with no
implementation impact ("what if the user has 10 million rows" on an internal tool
with 40 rows) — that is interrogation theater.

## Completeness gate (exit when ALL are true)

- [ ] Happy path is unambiguous end-to-end (no "somehow", "etc.", "and so on").
- [ ] Edge cases are enumerated, each with its expected behavior.
- [ ] Data shapes are concrete (fields + types), including what is optional.
- [ ] Failure policy is stated (fail loud vs degrade, retry vs abort, rollback).
- [ ] Out-of-scope list exists and the user has seen it.
- [ ] At least 3 acceptance checks are listed, each testable in one sentence.

## Output format (always end with this block)

```
SPEC — <feature name> — v1
Goal: <one sentence>
In scope: <bullets>
Out of scope: <bullets — explicit>
Data: <shapes, ownership, source of truth>
Edge cases: <case → expected behavior>
Failure policy: <what fails how, user-visible behavior>
Acceptance checks: <3+ verifiable one-liners>
Decisions made for you: <calls made on "you decide" answers>
Open risks: <anything accepted-but-unresolved, or "none">
```

## Anti-patterns (refuse these behaviors)

- Asking questions whose answer changes nothing in the code.
- A 4th round. If the gate still fails after 3 rounds, ship the spec with the
  gaps listed under **Open risks** and say so plainly.
- Padding the spec with sections the project does not need.
- Swallowing disagreement: if an answer contradicts an earlier one, surface the
  conflict immediately — do not silently pick one.
