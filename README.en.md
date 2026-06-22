# github-flow-skill

> A Claude Code skill that upgrades a bare `git push` into a proper GitHub
> collaboration flow. One command, `/github`, lights up all four dimensions of
> your contribution radar at once — **Commits / Issues / Pull requests / Code review**.

[中文文档 →](./README.md)

## What it is

Many people (especially on solo projects) just `git commit && git push` straight
to `main`. The result: the activity radar on their GitHub profile collapses into
a flat line — Commits at ~97%, Pull requests in the single digits, Issues and
Code review at zero.

This skill makes Claude Code run your current changes through the full flow
whenever you type `/github`:

```
1. Create an Issue        → counts toward Issues
2. Branch off
3. commit + push
4. Open a PR (Closes #N)  → counts toward Pull requests
5. Post a self-review     → counts toward Code review
6. Pause for your confirmation
7. squash merge + delete branch + auto-close the Issue → counts toward Commits
```

One pass, **all four corners +1**.

## Features

- **Fully CLI-based** via the [`gh` CLI](https://cli.github.com/) — no need to open the GitHub web UI.
- **Explicit trigger** — runs only when you type `/github`. Your everyday
  `git commit` / `git push` is never affected.
- **Brakes before merge** — branch, Issue, PR and self-review are automated, but
  the merge step always stops and waits for your confirmation.
- **Chinese-friendly** — generated Issue / PR / commit text is in Chinese by default.

## Install

Requires the [`gh` CLI](https://cli.github.com/), authenticated via `gh auth login`.

```bash
git clone https://github.com/superlls/github-flow-skill.git
cd github-flow-skill
ln -s "$(pwd)/skills/github" ~/.claude/skills/github
```

Restart Claude Code and type `/github`.

## License

MIT
