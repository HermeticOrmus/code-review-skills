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

---

## Part of the Libre Open-Source Stack for Claude Code

This repository is part of a growing family of open-source toolkits for Claude Code.

### Libre suite — comprehensive plugin bundles

- [LibreUIUX-Claude-Code](https://github.com/HermeticOrmus/LibreUIUX-Claude-Code) — UI/UX development (152 agents, 70 plugins, 76 commands, 74 skills)
- [LibreArch-Claude-Code](https://github.com/HermeticOrmus/LibreArch-Claude-Code) — Software architecture and system design
- [LibreCopy-Claude-Code](https://github.com/HermeticOrmus/LibreCopy-Claude-Code) — Technical writing and documentation engineering
- [LibreDevOps-Claude-Code](https://github.com/HermeticOrmus/LibreDevOps-Claude-Code) — DevOps engineering and infrastructure automation
- [LibreEmbed-Claude-Code](https://github.com/HermeticOrmus/LibreEmbed-Claude-Code) — Embedded systems, firmware, and IoT development
- [LibreFinTech-Claude-Code](https://github.com/HermeticOrmus/LibreFinTech-Claude-Code) — Financial technology development
- [LibreGEO-Claude-Code](https://github.com/HermeticOrmus/LibreGEO-Claude-Code) — AI-search optimization (ChatGPT, Perplexity, Gemini, Google AI Overviews)
- [LibreGameDev-Claude-Code](https://github.com/HermeticOrmus/LibreGameDev-Claude-Code) — Game development across Godot, Unity, Unreal
- [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code) — ML engineering and AI operations
- [LibreMobileDev-Claude-Code](https://github.com/HermeticOrmus/LibreMobileDev-Claude-Code) — Mobile app development (Flutter, React Native, native iOS, native Android)
- [LibreSecOps-Claude-Code](https://github.com/HermeticOrmus/LibreSecOps-Claude-Code) — Security operations
- [LibreSessionFlow-Claude-Code](https://github.com/HermeticOrmus/LibreSessionFlow-Claude-Code) — Session lifecycle: handoff, pickup, absorb, explore, close

### Skills mini-repos — single CLAUDE.md drop-ins

- [vibe-engineer-skills](https://github.com/HermeticOrmus/vibe-engineer-skills) — Direct AI codegen well: hypothesis before help, scoped prompts, validate before accepting
- [markdown-discipline-skills](https://github.com/HermeticOrmus/markdown-discipline-skills) — Strip AI-slop from markdown (no em dashes, no marketing fluff)
- [shell-safety-skills](https://github.com/HermeticOrmus/shell-safety-skills) — `set -euo pipefail` discipline plus 15 failure-mode examples
- [commit-standard-skills](https://github.com/HermeticOrmus/commit-standard-skills) — Ormus Commit Standard v1.0 plus commit-msg hook and commitlint
- [unwoke-skills](https://github.com/HermeticOrmus/unwoke-skills) — Strip AI theater (ten sins to eliminate, symmetric engagement)
- [python-conventions-skills](https://github.com/HermeticOrmus/python-conventions-skills) — Modern Python 3.11+ (types, pathlib, async, ruff, mypy, uv)
- [typescript-conventions-skills](https://github.com/HermeticOrmus/typescript-conventions-skills) — TypeScript strict mode, discriminated unions, Result types
- [hermetic-laws-skills](https://github.com/HermeticOrmus/hermetic-laws-skills) — Seven Hermetic Principles applied to engineering
- [riper-workflow-skills](https://github.com/HermeticOrmus/riper-workflow-skills) — Research / Innovate / Plan / Execute / Review systematic dev
- [six-day-cycle-skills](https://github.com/HermeticOrmus/six-day-cycle-skills) — Sustainable shipping cadence with mandatory rest
- [token-optimization-skills](https://github.com/HermeticOrmus/token-optimization-skills) — Claude Code token and context optimization
- [osint-skills](https://github.com/HermeticOrmus/osint-skills) — OSINT research methodology (multi-wave investigative spiral)
- [calcinate-skills](https://github.com/HermeticOrmus/calcinate-skills) — Stage 1 of the Magnum Opus (burn project bloat)
- [claude-md-overhaul-skills](https://github.com/HermeticOrmus/claude-md-overhaul-skills) — Audit CLAUDE.md and MEMORY.md against caps
- [session-handoff-skills](https://github.com/HermeticOrmus/session-handoff-skills) — Session handoff and pickup discipline
- [naming-skills](https://github.com/HermeticOrmus/naming-skills) — Product naming methodology (mine the brand's vocabulary)
- [magnum-opus-skills](https://github.com/HermeticOrmus/magnum-opus-skills) — Seven-stage alchemy applied to project transformation
- [mem-search-skills](https://github.com/HermeticOrmus/mem-search-skills) — Search claude-mem cross-session memory: search, filter, fetch
- [hypothesis-debugging-skills](https://github.com/HermeticOrmus/hypothesis-debugging-skills) — Hypothesis-driven debugging: reproduce, isolate, test, fix
- [vibe-proof-skills](https://github.com/HermeticOrmus/vibe-proof-skills) — Security hardening for vibe-coded full-stack apps
- [tdd-skills](https://github.com/HermeticOrmus/tdd-skills) — Test-driven development (Red-Green-Refactor) for JS/TS and Python
- [mars-skills](https://github.com/HermeticOrmus/mars-skills) — Production-readiness audit: the five mortal sins of vibe-coded MVPs
- [git-workflow-skills](https://github.com/HermeticOrmus/git-workflow-skills) — Clean git workflow: branch, atomic commits, reviewable PRs
- [code-comprehension-skills](https://github.com/HermeticOrmus/code-comprehension-skills) — Understand an unfamiliar codebase fast
- [dx-audit-skills](https://github.com/HermeticOrmus/dx-audit-skills) — Audit developer experience: docs, onboarding, tooling friction
- [setup-env-skills](https://github.com/HermeticOrmus/setup-env-skills) — Set up a project's development environment
- [automate-skills](https://github.com/HermeticOrmus/automate-skills) — Turn repetitive tasks into reliable automation scripts
- [quick-fix-skills](https://github.com/HermeticOrmus/quick-fix-skills) — Fast troubleshooting for common issues
- [prime-context-skills](https://github.com/HermeticOrmus/prime-context-skills) — Prime project context at the start of a session
- [auto-docs-skills](https://github.com/HermeticOrmus/auto-docs-skills) — Generate and maintain project documentation
- [learning-skills](https://github.com/HermeticOrmus/learning-skills) — Learn any technology: roadmaps, explanations, practice, cheatsheets, comparisons
- [linux-sysadmin-skills](https://github.com/HermeticOrmus/linux-sysadmin-skills) — Linux system administration: security, performance, diagnostics, monitoring, maintenance

### Template source

- [andrej-karpathy-skills](https://github.com/HermeticOrmus/andrej-karpathy-skills) — the canonical single-file CLAUDE.md pattern (fork of jiayuan_jy's original)

Star the family, not just one — that's how the suite stays coherent.
