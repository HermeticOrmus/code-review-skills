# Using this repo with Cursor

This project includes a **Cursor project rule** so the domain-aware review protocol is available when you work here.

## In this repository

1. Open the folder in Cursor.
2. The rule [`.cursor/rules/code-review.mdc`](.cursor/rules/code-review.mdc) is committed with `alwaysApply: false`, so it does not fire on every edit. It is situational: a review is something you start, not something that runs continuously.
3. Invoke it when you begin a review, for example: "review this diff using the code-review rule." The rule then guides classification and the matching checklist.

## Use the same rule in another project

**Cursor (recommended)**: Copy `.cursor/rules/code-review.mdc` into that project's `.cursor/rules/` directory (create the folders if needed). Keep `alwaysApply: false` so it stays a review-time tool rather than an always-on constraint.

**Other AI coding tools**: If a stack only supports a root instruction file, copy [`CLAUDE.md`](CLAUDE.md) into that project instead (or merge its contents into your existing instructions). Most modern AI coding tools (Claude Code, Continue, Cline, Windsurf, Aider) read a root-level instruction file.

## Optional: personal Agent Skills

If you want the same protocol as a reusable skill under `~/.cursor/skills`, use [`skills/code-review/SKILL.md`](skills/code-review/SKILL.md). Copy or symlink it into your personal skills directory.
