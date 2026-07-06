---
name: changelog-writer
description: Turn a set of changes (commits/PRs/diff) into a clean changelog or release notes, grouped and written for the reader. Use when cutting a release or when asked "write the changelog", "release notes for this version", "summarize what changed". Keep-a-Changelog style, human-first.
---

# Changelog Writer

A changelog is for the human deciding whether to upgrade — not a git log dump. Group by
impact, lead with what breaks, write in plain language.

## Inputs

Commits / merged PRs / the diff since the last tag. Read them; don't trust subjects alone
if the diff tells a different story.

## Grouping (Keep a Changelog)

Under a version header (`## [x.y.z] - YYYY-MM-DD`, date passed in — don't guess it):
- **Breaking changes** — FIRST, always. What broke + the migration step.
- **Added** — new features/capabilities the user can now use.
- **Changed** — behavior changes that aren't breaking.
- **Fixed** — bug fixes, described by the symptom the user saw ("fixed crash when…").
- **Deprecated** / **Removed** / **Security** — as applicable.

Drop internal noise: refactors, test-only changes, CI, dependency bumps with no user
impact. If a commit doesn't change what the user experiences, it doesn't belong here.

## Writing

- One line per entry, past tense, user-visible outcome first:
  "Fixed export failing on files over 2 GB" — not "patch bufferSize in exporter.ts".
- Link the PR/issue if available. Credit external contributors.
- Breaking entries carry a one-line "→ migrate: …".

## Versioning hint (semver)

Any breaking change → major. New backward-compatible feature → minor. Only fixes → patch.
Flag if the requested version number disagrees with the changes.

## Output

The version block, ready to prepend to CHANGELOG.md, plus (optional) a 2-3 sentence
release summary for an announcement.
