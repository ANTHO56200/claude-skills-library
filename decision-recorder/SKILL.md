---
name: decision-recorder
description: Capture a technical or architectural decision as a clear, durable record (ADR) so the reasoning survives. Use after making a non-trivial choice (framework, pattern, trade-off), or asked "write an ADR", "document this decision", "record why we chose X". Captures the WHY and the alternatives, not just the what.
---

# Decision Recorder (ADR)

The most expensive lost information in a codebase is WHY a decision was made. Six months
later someone asks "why is it like this?" and no one remembers — so they either cargo-cult it
or break it. An ADR (Architecture Decision Record) preserves the reasoning.

## Capture what actually matters

A decision is only useful with its context and its alternatives. Record:

- **Title + status**: what was decided, in a phrase; status (proposed / accepted / superseded).
- **Context**: the situation and forces that made a decision necessary — the constraints,
  requirements, and problem at the time. (Decisions are right or wrong relative to their context;
  without it, the future reader can't judge whether it still holds.)
- **Decision**: what was chosen, stated plainly.
- **Alternatives considered**: the other real options and WHY they were rejected. This is the
  highest-value part — it stops the future team from re-litigating the same options, or reviving
  a rejected one without knowing it was already weighed.
- **Consequences**: what this makes easier AND harder — the trade-off accepted, the new
  constraints, what to watch for. Every decision has a cost; name it honestly.

## Keep it durable

- **Immutable + append-only**: don't rewrite history. If a decision is reversed, write a NEW
  record that supersedes the old one (and link them). The trail of how thinking evolved is itself valuable.
- Short and specific — one decision per record. A vague ADR helps no one.
- Store it with the code (a `docs/adr/` folder), dated, numbered.

## Rules

- The WHY and the rejected alternatives are the point — the "what" is visible in the code already.
- Record the context of the moment; a decision without its constraints can't be re-evaluated later.
- Name the trade-off honestly; a decision with only upsides listed is a sales pitch, not a record.
- Supersede, don't edit; the evolution of the reasoning is history worth keeping.

## Output

The ADR (title/status/context/decision/alternatives-with-rejection-reasons/consequences), ready
to drop into the repo. One decision, dated, with the trade-off named.
