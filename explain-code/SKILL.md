---
name: explain-code
description: Explain what a piece of code, a function, or a codebase actually does — clearly, at the right depth. Use when onboarding to unfamiliar code, understanding a legacy function, or asked "explain this code", "what does this do", "walk me through this file/repo". Explains the WHY and the flow, not line-by-line noise.
---

# Explain Code

Good code explanation gives understanding you can act on — the purpose, the flow, and the
gotchas — not a translation of each line into prose. Read the real code; don't guess from names.

## Explain at the right altitude (match the ask)

- **"What is this repo?"** → the big picture first: what it does, entry points, how the main
  pieces fit, the tech/architecture. Don't dive into a random file.
- **"What does this function do?"** → its job in one sentence, then inputs → what it does →
  outputs/side effects, then the non-obvious parts (edge cases, why that weird bit exists).
- **"Walk me through this file"** → the flow: what calls what, the data's journey, the key
  decisions — not every line.

## What actually helps

- **Purpose before mechanism**: WHY this exists / what problem it solves, then HOW. Mechanism
  without purpose is just narration.
- **The flow / data journey**: where input enters, how it transforms, where it exits. Control
  flow and data flow are what people actually need to hold in their head.
- **The gotchas**: the non-obvious dependency, the implicit assumption, the "if you change this,
  that breaks", the historical weirdness. This is the highest-value part — it's what a newcomer can't see.
- **Concrete example**: trace one real input through it. An example teaches faster than abstraction.

## Rules

- Don't restate obvious lines ("this loop iterates over the users") — that adds nothing.
- Don't guess. If a name is misleading or intent is unclear, read what it actually does and
  say so ("despite the name, this doesn't cache — it…") — never invent a rationale.

## Output

An explanation at the altitude asked: purpose → flow → gotchas, with a concrete example if it
helps. Flag anything that looks like a bug or a landmine you noticed while reading.
