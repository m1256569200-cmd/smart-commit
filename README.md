# smart-commit

An agent skill that turns your `git diff` into a clean [Conventional Commits](https://www.conventionalcommits.org/) message. Drop it into your agent's skills directory and never write a sloppy commit message again.

## Install

Copy `SKILL.md` into your skills folder, e.g.:

- Claude Code: `~/.claude/skills/smart-commit/SKILL.md`
- Cursor: `.cursor/skills/smart-commit/SKILL.md` (project) or `~/.cursor/skills/` (global)

Then just ask your agent: "commit these changes" or "write a commit message".

## What it does

1. Inspects staged (or unstaged) changes — never guesses from filenames.
2. Picks the right `type` (`feat`, `fix`, `docs`, …) and `scope`.
3. Outputs a copy-pasteable message in a code block.
4. Suggests splitting the commit when the diff mixes unrelated changes.

It will not run `git commit` for you unless you ask.

## Example

```
feat(auth): add OAuth2 refresh token rotation

Rotates refresh tokens on every use and invalidates the previous
one to reduce the impact of token theft.
```

## License

MIT
