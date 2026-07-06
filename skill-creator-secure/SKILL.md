---
name: skill-creator-secure
description: Write a new Claude/agent skill correctly and safely — tight SKILL.md, token budget, and a built-in security lint of the result. Use when the user wants to create, write, or scaffold a new skill ("make a skill for X", "write a SKILL.md", "turn this workflow into a skill").
---

# Skill Creator (secure)

Build a skill that's focused, token-lean, and safe — then lint what you produced. A skill
is an instruction the model will follow with tools; sloppy or unsafe skills do real harm.

## Anatomy (get this exactly right)

```
---
name: kebab-case-name
description: <what it does + WHEN to use it — trigger phrases. This is what routes the skill.>
---
# Title
<the instructions — imperative, specific, the model is the reader>
```

- **description** decides when the skill fires — pack it with concrete trigger cases, not fluff.
- Body: imperative rules, not prose essays. Show the exact checks/format/output.
- Bundle extra files (scripts, templates) only if used; reference them by relative path.

## Design rules

- **One job.** A skill that does five things fires at the wrong time and bloats context.
  Split it. (Monolithic skills measurably lose accuracy.)
- **Token budget.** Aim for a lean body (a few hundred tokens). Every token loads into
  context on trigger. If it's long, it's probably two skills or padded.
- **Deterministic where possible.** Prefer a runnable check/regex/gate over "be careful".
- **Anti-patterns section.** State what the skill must NOT do — that's where safety lives.

## Security lint (run on the skill you just wrote — don't ship without it)

- No instruction-override / "hide from user" phrasing.
- No unpinned remote fetch piped to a shell, no exfiltration, no destructive command by default.
- No request for broader tools than the job needs.
- Bundled scripts: no secrets, no `eval`, no `curl | sh`.
(If it would fail `skill-security-auditor`, fix it before shipping.)

## Output

The complete SKILL.md (+ any bundled files), the estimated token count, and a one-line
"lint: clean" or the issues found and fixed.

## Rules

- Write for the model, not for a human reader — it's an instruction set, not a tutorial.
- If the "skill" is really just a prompt with no reusable rules, say so — not everything needs to be a skill.
