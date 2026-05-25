<p align="center">
  <img src="https://ormus.solutions/mascot/pixellab_liquid_to_eye.gif" alt="Code Review Skills" width="128" style="image-rendering: pixelated;" />
</p>

<h1 align="center">Code Review Skills</h1>

<p align="center">
  <em>A Claude Code skill for domain-aware code review — classify the code, then review for the failure modes that domain actually has.</em>
</p>

<p align="center">
  <a href="https://github.com/HermeticOrmus/code-review-skills/stargazers"><img src="https://img.shields.io/github/stars/HermeticOrmus/code-review-skills?style=flat-square&color=aa8142" alt="Stars" /></a>
  <a href="https://github.com/HermeticOrmus/code-review-skills/blob/main/LICENSE"><img src="https://img.shields.io/github/license/HermeticOrmus/code-review-skills?style=flat-square&color=aa8142" alt="License" /></a>
  <a href="https://github.com/HermeticOrmus/code-review-skills/commits"><img src="https://img.shields.io/github/last-commit/HermeticOrmus/code-review-skills?style=flat-square&color=aa8142" alt="Last Commit" /></a>
  <img src="https://img.shields.io/badge/Claude_Code-aa8142?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code" />
</p>

---

## The problem

A single review checklist applied to every change wastes attention on checks that do not apply and misses the ones that do. Reviewing an API endpoint for complexity, or a sorting routine for rate limiting, asks the wrong questions. The failure modes that matter depend on what kind of code it is.

A complexity bug hides in algorithm code. A missing-validation hole hides in an API handler. A race hides in concurrent code. An N+1 hides in a data-access loop. One generic pass over all of it tends to surface formatting nits and miss the failure mode that ships the incident.

The fix is to classify the change first, then review for the failure modes that domain actually has.

## Domain to focus

| Domain | Indicators | Review focus |
|---|---|---|
| Algorithm | Loops, recursion, data structures, sorting or searching | Time and space complexity, edge cases, correctness |
| API or network | HTTP, requests, endpoints, REST, GraphQL | Error handling, rate limiting, auth, validation |
| Security | Auth, crypto, passwords, tokens, user input | OWASP top 10, injection, data exposure |
| Data | SQL, ORM, database, queries, transactions | N+1, indexes, consistency, migrations |
| UI or frontend | React, components, DOM, CSS, events | Accessibility, performance, state management |
| Infrastructure | Docker, K8s, CI/CD, configs, scripts | Security, idempotency, failure modes |

A change can span domains. Run each matching checklist; do not pick one silently.

Full protocol: [`CLAUDE.md`](CLAUDE.md). Worked reviews: [`EXAMPLES.md`](EXAMPLES.md).

## Install

### As a project CLAUDE.md

Drop [`CLAUDE.md`](CLAUDE.md) at the root of your repository. Claude Code picks it up automatically. Merge with existing project instructions if any.

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/HermeticOrmus/code-review-skills/main/CLAUDE.md
```

### As a Claude Code skill

The same protocol is packaged as a skill under [`skills/code-review/`](skills/code-review/) for `~/.claude/skills/`. See the `SKILL.md` inside for installation.

### As a `/review` slash command

Save the protocol as a personal slash command so you can invoke it on a target directly:

```bash
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/review.md https://raw.githubusercontent.com/HermeticOrmus/code-review-skills/main/skills/code-review/SKILL.md
```

Then run `/review <file-or-diff>` in any Claude Code session.

### In Cursor

See [`CURSOR.md`](CURSOR.md) for the Cursor-rule equivalent at [`.cursor/rules/code-review.mdc`](.cursor/rules/code-review.mdc). It is a situational rule (`alwaysApply: false`); invoke it when you start a review.

### In other AI coding tools

If your tool reads a single instruction file at the project root, copy `CLAUDE.md` to whatever name your tool expects (`AGENTS.md`, `INSTRUCTIONS.md`, etc.).

## See also

This is the everyday diff and pull-request review. Two specialized deep-dives go further when the situation calls for it, and one covers the human side of the work.

- [`mars-skills`](https://github.com/HermeticOrmus/mars-skills): production-readiness audit, the harder pass for code about to ship, hunting the gap between "works on my machine" and battle-tested.
- [`vibe-proof-skills`](https://github.com/HermeticOrmus/vibe-proof-skills): security hardening for full-stack apps, covering injection, PII exposure, missing headers, error leakage, credential hygiene.
- [`vibe-engineer-skills`](https://github.com/HermeticOrmus/vibe-engineer-skills): the human-side discipline for directing AI codegen, which produces much of the code you now review.

## Contributing

PRs welcome, especially for additional worked reviews in [`EXAMPLES.md`](EXAMPLES.md), new domain rows for the focus table (mobile, ML, embedded, game loops), and adaptations of `CURSOR.md` for other AI coding tools.

## License

MIT. Use it, fork it, merge it into your own CLAUDE.md.
