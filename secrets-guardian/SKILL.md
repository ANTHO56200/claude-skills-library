---
name: secrets-guardian
description: Detect and prevent secrets (API keys, tokens, private keys, credentials) from entering code, commits, logs, or config. Use before committing, when adding config, when handling credentials, or on request ("check for leaked keys", "scan for secrets"). Catches leaks and proposes the safe pattern.
---

# Secrets Guardian

Secrets in a repo are forever (history, forks, mirrors, model training crawls).
Catch them before they land, and fix the pattern that let them in.

## Detect (scan added/staged lines)

High-signal patterns:
- `sk-[A-Za-z0-9]{20,}`, `ghp_[A-Za-z0-9]{36}`, `xox[baprs]-[A-Za-z0-9-]+` (Slack),
  `AKIA[0-9A-Z]{16}` (AWS), `AIza[0-9A-Za-z_-]{35}` (Google).
- `-----BEGIN (RSA |EC |OPENSSH )?PRIVATE KEY-----`.
- Generic: `(password|passwd|secret|token|api[_-]?key)\s*[:=]\s*['"][^'"\s]{8,}['"]`
- Connection strings with inline creds: `(postgres|mysql|mongodb)://[^:]+:[^@]+@`.
- High-entropy string assigned to a suspicious name (base64/hex ≥ 32 chars).

## Triage a hit

1. **Real secret?** — a live-looking key/token/password → CRITICAL, stop.
2. **Placeholder / example?** — `your-key-here`, `xxxx`, `example`, obvious dummy → OK, but
   move it to `.env.example` so it never sits next to real config.
3. **False positive?** — a variable *name* like `apiKey` with no value, or a public
   identifier → fine; don't cry wolf.

## Fix pattern (never just delete)

- Move the value to an environment variable; reference `process.env.X` / `os.environ`.
- Add the key (name only) to `.env.example`; ensure `.env` is git-ignored.
- If it was ever committed: the key is **compromised** — rotate it at the provider,
  then scrub history. Rotation first; a deleted-but-already-pushed key is still burned.
- Add a pre-commit guard so it can't recur.

## Output

```
SECRETS SCAN — <target>
Findings: <n critical / n review / n ignored-fp>
  [CRITICAL] <file:line> — <type> — ROTATE + remove + move to env
Safe pattern applied: <what changed>
```

## Rules

- Assume any matched real secret is already compromised the moment it's in history — advise rotation, always.
- Do not print the full secret back in output/logs — mask the middle.
- Distinguish name-only mentions from real values; false alarms train people to ignore you.
