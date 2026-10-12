---
name: code-review
description: Perform a structured code review of a diff, branch, or pull request. Use when the user asks to review code, check a PR, or run this skill before merging.
---

# Code Review

Review code like a senior engineer: find real problems, skip the noise.

## Workflow

1. Get the diff. Prefer `git diff <base>...<head>` for branches, or the PR diff. For uncommitted work, use `git diff`.
2. Understand intent first: read the surrounding code and, if available, the linked issue or PR description. Don't flag style in code you don't understand yet.
3. Review in three passes, in this order:
   - **Correctness**: logic errors, off-by-one, null/undefined handling, race conditions, error paths that swallow exceptions.
   - **Security**: injection (SQL, shell, template), auth checks on new endpoints, secrets in code, unsafe deserialization, overly broad permissions.
   - **Maintainability**: only flag what genuinely hurts future readers — dead code, misleading names, duplicated logic worth extracting.
4. Output format — group findings by severity:

```
## 🔴 Blockers (must fix)
- `path/to/file.ts:42` — description + why it breaks + suggested fix

## 🟡 Suggestions (consider)
- `path/to/file.ts:87` — description + suggestion

## 🟢 Nits (optional, batch them)
- ...
```

If there is nothing wrong, say so in one line and name what you checked. Never invent problems to fill a quota.

## Rules

- Every finding must cite a file and line. No vague "consider improving error handling".
- Do not comment on formatting — linters own that.
- Do not request changes for personal taste; tie each blocker to correctness or security.
- Keep it scannable: short bullets, code refs, no essays.
