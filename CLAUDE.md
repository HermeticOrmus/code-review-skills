# CLAUDE.md

A review protocol for the everyday diff or pull request. Classify the code's domain first, then review for the failure modes that domain actually has. Merge with project-specific instructions as needed.

**Tradeoff**: one checklist for all code wastes attention on irrelevant checks and misses the ones that matter. Classifying first costs a moment and focuses the rest of the review where the real risk lives.

## 1. Review the change, not the file

When reviewing a change, read the diff, not the whole file.

- The author is responsible for what they touched. Review that.
- Read enough surrounding context to judge the change's correctness, no more.
- A finding outside the diff is worth noting only if the change makes it newly reachable or newly wrong.
- Whole-file review is a separate task with a different goal. Don't conflate them.

## 2. Classify the code domain

Read the target and place it in a domain before forming any opinion. The domain decides which failure modes are plausible.

| Domain | Indicators | Review focus |
|---|---|---|
| Algorithm | Loops, recursion, data structures, sorting or searching | Time and space complexity, edge cases, correctness |
| API or network | HTTP, requests, endpoints, REST, GraphQL | Error handling, rate limiting, auth, validation |
| Security | Auth, crypto, passwords, tokens, user input | OWASP top 10, injection, data exposure |
| Data | SQL, ORM, database, queries, transactions | N+1, indexes, consistency, migrations |
| UI or frontend | React, components, DOM, CSS, events | Accessibility, performance, state management |
| Infrastructure | Docker, K8s, CI/CD, configs, scripts | Security, idempotency, failure modes |

A change can span domains. Apply each matching checklist; do not pick one silently.

## 3. Apply the matching focus checklist

Once the domain is set, review against the checklist for that domain.

### Algorithm code

1. Correctness: does it solve the problem for all inputs, not just the example?
2. Complexity: what is the Big-O? Is there a hidden quadratic in a nested loop or a repeated linear scan?
3. Edge cases: empty input, single element, maximum size, duplicate keys, negative numbers.
4. Readability: do the variable names and control flow make the invariant legible?

### API or network code

1. Error handling: what happens on timeout, 4xx, 5xx, connection reset, partial response?
2. Validation: is input validated and typed before it reaches business logic?
3. Authentication and authorization: is the endpoint protected, and does it check the caller may act on this resource?
4. Rate limiting and retries: is there protection against abuse, and are retries idempotent?

### Security-sensitive code

1. Input validation: is every untrusted input sanitized at the boundary?
2. Injection: SQL, command, XSS, template, deserialization?
3. Secrets: are credentials hardcoded, logged, or returned in a response?
4. Access control: is authorization enforced server-side, not just hidden in the UI?

### Data or database code

1. N+1 queries: is there a query inside a loop that could be one join or batch?
2. Transactions: are related writes atomic, and is the isolation level correct for the read pattern?
3. Indexes: will the query use an index, or is it a full scan at production row counts?
4. Migrations: is the schema change backward compatible with the currently deployed code?

### UI or frontend code

1. Accessibility: keyboard reachable, labelled, sufficient contrast, semantic markup?
2. Performance: unnecessary re-renders, unbounded lists, layout thrash, blocking work on the main thread?
3. State: is derived state recomputed instead of stored, and are loading, empty, and error states handled?
4. Correctness: are effects cleaned up, and are race conditions between async updates handled?

### Infrastructure code

1. Security: least privilege, no secrets in the image or config, network surface minimized?
2. Idempotency: can the script or apply run twice without harm?
3. Failure modes: what happens on partial apply, and is there a rollback path?
4. Observability: will a failure be visible, or will it fail silently?

## 4. Assign severity to every finding

A finding without a severity is a finding the author cannot triage. Use three levels.

- **Critical (must fix)**: wrong output, data loss, security hole, broken contract. Blocks merge.
- **Improvement (should fix)**: correct but fragile, slow at scale, missing an edge case the change makes reachable. Fix now or file it.
- **Nitpick (optional)**: naming, style, a clearer phrasing. Author's discretion.

If you cannot assign a severity, the finding is not yet specific enough to report.

## 5. Structure the feedback

Lead with what the change does, name the domain, then the findings ordered by severity. End with what is correct so the author knows it was actually read.

```text
## Summary
[1-2 sentence overview of what the change does]

## Domain: [detected domain(s)]

## Critical (must fix)
- [ ] [file:line] Description + concrete suggestion

## Improvements (should fix)
- [ ] [file:line] Description + concrete suggestion

## Nitpicks (optional)
- [ ] [file:line] Description

## What's correct
- Observation the author got right
```

Every finding cites a location. Every finding proposes a direction, not just a complaint.

---

## Trigger this protocol when

- Reviewing a diff or pull request → classify before commenting
- A review comment has no severity → assign one or drop it
- A review wanders outside the diff → pull it back unless the change made it reachable
- Code spans domains → run each matching checklist, do not pick one
- A finding is vague ("this feels off") → make it specific or do not report it

---

## See also

For the everyday review this file is enough. Two specialized deep-dives go further when the situation calls for it.

- [`mars-skills`](https://github.com/HermeticOrmus/mars-skills): production-readiness audit, the harder pass for code about to ship, hunting the gap between "works on my machine" and battle-tested.
- [`vibe-proof-skills`](https://github.com/HermeticOrmus/vibe-proof-skills): security hardening for full-stack apps, covering injection, PII exposure, missing headers, credential hygiene.
- [`vibe-engineer-skills`](https://github.com/HermeticOrmus/vibe-engineer-skills): the human-side discipline for directing AI codegen, which produces much of the code you now review.

**License**: MIT. Use it, fork it, merge it into your own.
