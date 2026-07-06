---
name: readme-writer
description: Write or update a README that a newcomer can actually follow, grounded in the real code. Use when a project needs a README, the README is stale, or the user says "write a readme", "document this project". Reads the code first; updates incrementally instead of regenerating.
---

# README Writer

A README earns its place by getting a stranger from zero to running in minutes. Ground
every claim in the actual code — a README that lies is worse than none.

## Read before you write

- Entry points, scripts (`package.json`/`Makefile`/`pyproject`), config, env vars needed.
- What it actually does (don't guess from the name), and what it does NOT do.
- Real install/run/test commands — copy them from the code, don't invent flags.

## Structure (include what applies, skip what doesn't)

1. **One-liner**: what it is + who it's for, in a sentence.
2. **Why / what it does**: the problem it solves; key features as short bullets.
3. **Quickstart**: install → configure (env vars, with an `.env.example` pointer) → run.
   Commands must be copy-pasteable and actually work.
4. **Usage**: the 2-3 most common real examples, with expected output.
5. **Configuration**: env vars / options table — name, purpose, default.
6. **Development**: how to run tests, project layout if non-obvious.
7. **License / links**: only if they exist.

## Update mode (default when a README exists)

- Change only what drifted (new script, renamed command, new env var, removed feature).
- Preserve the author's voice, ordering, and hand-written sections. Don't regenerate wholesale.
- Fact-check existing claims against the code; fix the ones that are now wrong.

## Rules

- No feature the code doesn't have. No command you haven't traced to a script. Verify, don't hallucinate.
- Keep it scannable: short sections, real code blocks, no marketing fluff.
- Badges and diagrams only if they carry information, not decoration.
