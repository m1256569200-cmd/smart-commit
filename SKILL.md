---
name: smart-commit
description: Generate a Conventional Commits message from staged changes or a diff. Use when the user asks to commit, write a commit message, or run this skill before committing.
---

# Smart Commit

Turn `git diff` output into a clean [Conventional Commits](https://www.conventionalcommits.org/) message.

## Workflow

1. Run `git diff --cached --stat` first. If nothing is staged, run `git diff --stat` and tell the user which files the message will cover (unstaged changes).
2. Read the actual diff (`git diff --cached`, limited to a reasonable size). Never guess from filenames alone.
3. Write the message in this format:

```
<type>(<scope>): <short imperative summary>

<body — what changed and why, 1-3 lines, only if non-obvious>
```

4. Rules:
   - `type`: feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert. Pick exactly one.
   - `scope`: the module, file, or area touched (e.g. `auth`, `api`, `readme`). Omit if the change spans the whole repo.
   - Summary: imperative mood ("add", not "added"), ≤ 72 chars, no trailing period.
   - If the diff mixes unrelated changes, propose splitting into multiple commits instead of one message.
5. Output ONLY the commit message in a code block so it can be copied. Do not run `git commit` unless the user explicitly asks.

## Examples

```
feat(auth): add OAuth2 refresh token rotation

Rotates refresh tokens on every use and invalidates the previous
one to reduce the impact of token theft.
```

```
fix(api): handle empty query param in /search

Previously returned 500 when `q` was missing; now returns 400
with a clear error message.
```
