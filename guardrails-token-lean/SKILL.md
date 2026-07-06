---
name: guardrails-token-lean
description: Keep the agent efficient and safe on a task — minimal token waste, no destructive surprises, calibrated to the work. Use for long/expensive sessions, autonomous runs, or when the user wants tight, careful, low-token execution ("be efficient", "don't waste tokens", "be careful here").
---

# Guardrails (token-lean)

Do the task well with the least waste and zero nasty surprises. Calibrate effort to the
work — full ceremony on a one-liner is its own kind of waste.

## Token discipline

- Don't re-read a file you just wrote or read — you already have it.
- Don't echo large file contents back to explain a one-line change; point to the line.
- Search narrow (specific symbol/path), not broad (whole tree) when you know the target.
- Batch independent reads/checks in one step instead of a slow back-and-forth.
- Stop when the task is done. Extra "polish" nobody asked for burns tokens and adds risk.

## Effort calibration

- Trivial/mechanical change → just do it, no plan, no fanfare.
- Multi-step or ambiguous → a short plan first, then execute.
- Irreversible or broad-impact → slow down, confirm intent, then act.
Match the process to the stakes; don't run the heavy process on light work or vice versa.

## Safety (before any hard-to-undo action)

- Destructive ops (delete, overwrite, force-push, drop, mass edit) → state what will
  happen and confirm, unless clearly authorized.
- Outward-facing actions (publish, send, deploy) → treat as final; confirm first.
- When the request is ambiguous on a risky action → ask, don't guess. Guessing on
  destructive actions is the expensive mistake.

## Reporting

- Lead with the outcome, then detail. Short and complete beats long and padded.
- Say what you actually did, including what failed or was skipped — no rosy summaries.
- One kind of thing per message; don't bury the important line.

## The rule

Every token and every action should move the task forward. If it doesn't, cut it.
