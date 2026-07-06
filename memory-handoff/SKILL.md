---
name: memory-handoff
description: Persist project context between sessions with zero infrastructure — a file-based memory the next session (or agent) can pick up cold. Use when wrapping a session, handing off work, or asked "save context", "remember this", "write a handoff", "where was I". No database, no webhook.
---

# Memory / Handoff (zero-infra)

Give the next session (yours or a teammate's) what it needs to continue without re-deriving
everything. Plain files in the repo — no service, no daemon, loaded on demand only.

## What to persist (and what NOT)

Persist what is NOT recoverable from the code or git:
- **Decisions + the WHY** ("chose X over Y because Z") — the reasoning dies otherwise.
- **Non-obvious constraints** (this API rate-limits at N; that field is legacy; don't touch W).
- **In-flight state**: what's done, what's next, what's blocked and on what.
- **Gotchas discovered the hard way** (the bug that ate an hour, the flaky test's real cause).

Do NOT persist what the repo already records: file structure, what a function does, commit
history, anything a `grep` answers. Duplicated memory rots and misleads.

## Format (one fact per file, or one running handoff)

```
# Handoff — <date>
## Done
- <shipped, with where>
## Next (priority order)
- <the very next action, concrete>
## Blocked
- <what + on whom/what>
## Decisions
- <decision> — because <why>
## Gotchas
- <trap> → <how to avoid>
```

Keep an index file (one line per note) so recall is cheap. Convert relative dates to
absolute ("tomorrow" → the date). Link related notes by name.

## On resume (read this first)

Load the index → read the handoff → verify anything it names still exists in the code
before acting on it (memory reflects when it was written, not now). Then continue.

## Rules

- One fact per note; a note that says five things is impossible to update or trust.
- Update the note when the fact changes; delete it when it's wrong. Never let stale memory accumulate.
- Never store secrets in memory files (they're in the repo/history — see secrets-guardian).
