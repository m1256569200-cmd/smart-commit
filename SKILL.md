---
name: changelog-gen
description: Generate a changelog entry from recent commits or a diff range. Use when the user asks to update the changelog, write release notes, or summarize what changed.
---

# Changelog Gen

Turn git history into a human-readable changelog entry.

## Workflow

1. Determine the range. Ask the user if unclear; sensible defaults:
   - Since last tag: `git log $(git describe --tags --abbrev=0)..HEAD --oneline`
   - Unreleased only: commits not yet in the changelog.
2. Read the full messages (`git log <range> --format='%h %s%n%b'`), not just the one-liners. Group by intent, not by commit:
   - **Added** — new features (`feat`)
   - **Fixed** — bug fixes (`fix`)
   - **Changed** — behavior changes, refactors, perf
   - **Docs / Chores** — fold into one line unless notable; never list every chore commit individually.
3. Write entries for humans, not commit hashes:
   - One bullet per user-visible change. Merge duplicates ("fix login" × 3 → one bullet).
   - Past tense, plain language: "Fixed crash when uploading files over 100MB" — not "fix: upload bug".
   - Link PR/issue numbers when present in the messages (`#123`).
4. Output a Markdown section ready to paste under an `## [Unreleased]` or version heading:

```markdown
## [1.4.0] - 2026-10-12

### Added
- OAuth2 refresh token rotation (#128)

### Fixed
- Crash when uploading files over 100MB (#131)
- Search returning 500 on empty query (#129)
```

## Rules

- Skip merge commits and reverts unless the revert itself is user-visible.
- If the range has 50+ commits, summarize themes instead of listing everything, and say you did.
- Do not edit CHANGELOG.md in place unless the user asks; output the section.
