# Examples

Worked reviews across different domains. Each shows the classification step first, then the targeted findings that classification leads to. The point is that the domain decides which questions get asked.

---

## 1. An algorithm with a complexity bug

### The change

```python
def find_duplicates(records):
    duplicates = []
    for i in range(len(records)):
        for j in range(len(records)):
            if i != j and records[i].email == records[j].email:
                if records[i] not in duplicates:
                    duplicates.append(records[i])
    return duplicates
```

### Classify

Nested loops over a list, comparing every element to every other, plus a membership test. This is algorithm code. Focus: complexity, edge cases, correctness.

### Review

```text
## Summary
Collects records that share an email with another record.

## Domain: algorithm

## Critical (must fix)
- [ ] [find_duplicates] This is O(n^2) from the nested loop, and the
      `records[i] not in duplicates` membership test on a list adds another
      O(n) inside it, making it O(n^3) worst case. At a few thousand records
      this stalls. Build a dict of email -> count in one pass, then collect
      records whose email count > 1. That is O(n).

## Improvements (should fix)
- [ ] [find_duplicates] Each duplicate's first occurrence is dropped: the
      `i != j` check means a record only enters `duplicates` when a later
      copy exists, but ordering means some pairs add only one of the two.
      Decide whether all members of a duplicate group should be returned and
      make it explicit.

## Nitpicks (optional)
- [ ] Name reads as "records that are duplicates" but returns one entry per
      colliding record; `records_with_duplicate_emails` is clearer.

## What's correct
- Comparison key (email) is the right field for the stated intent.
```

The complexity finding is the one that matters, and classifying as algorithm code is what put complexity at the top of the list instead of, say, naming style.

---

## 2. An API endpoint missing validation

### The change

```js
app.post('/api/transfer', async (req, res) => {
  const { fromAccount, toAccount, amount } = req.body;
  await db.accounts.decrement(fromAccount, amount);
  await db.accounts.increment(toAccount, amount);
  res.json({ ok: true });
});
```

### Classify

HTTP endpoint, request body, database writes, money movement. This is API or network code with a security and data overlap. Focus: validation, auth, error handling, transactional integrity. Run all three matching checklists; do not pick one.

### Review

```text
## Summary
Adds a POST /api/transfer endpoint that moves an amount between two accounts.

## Domain: API or network (with security and data overlap)

## Critical (must fix)
- [ ] [transfer:auth] No authentication or authorization. Any caller can move
      money between arbitrary accounts. The handler never checks that the
      authenticated user owns `fromAccount`. This is the headline issue.
- [ ] [transfer:validation] `amount` is taken from the body unvalidated. A
      negative `amount` reverses the transfer direction; a non-numeric or
      missing value corrupts both balances. Validate: present, numeric,
      positive, within a sane bound, and that the source has sufficient funds.
- [ ] [transfer:atomicity] The decrement and increment are two separate awaits
      with no transaction. If the process dies or the second write fails, money
      vanishes from one account without arriving in the other. Wrap both in a
      single database transaction that rolls back on any failure.

## Improvements (should fix)
- [ ] [transfer:errors] No error handling. A rejected promise produces an
      unhandled rejection and a hung request instead of a 4xx/5xx with a body.
- [ ] [transfer:idempotency] No idempotency key. A client retry after a
      timeout double-applies the transfer. Accept and dedupe on a client token.

## Nitpicks (optional)
- [ ] `{ ok: true }` carries no transfer id; returning one aids client logging.

## What's correct
- Destructuring the three fields up front keeps the handler readable.
```

Note that classifying the endpoint surfaced the auth hole and the missing transaction. A generic style pass would likely have stopped at "add error handling."

---

## 3. A concurrency race

### The change

```go
var cache = map[string]*User{}

func GetUser(id string) (*User, error) {
    if u, ok := cache[id]; ok {
        return u, nil
    }
    u, err := db.LoadUser(id)
    if err != nil {
        return nil, err
    }
    cache[id] = u
    return u, nil
}
```

### Classify

Shared mutable map read and written from a handler that runs per request, so concurrent goroutines. This is concurrency code. Focus: data races, atomicity of read-modify-write, lock discipline.

### Review

```text
## Summary
Adds a process-wide cache in front of db.LoadUser, keyed by user id.

## Domain: concurrency

## Critical (must fix)
- [ ] [GetUser] Data race. `cache` is a plain map read at the lookup and
      written at `cache[id] = u`, with no synchronization. Go maps are not
      safe for concurrent read and write; concurrent requests will trigger a
      fatal "concurrent map read and map write" and crash the process. Guard
      with a sync.RWMutex (RLock on read, Lock on write) or use sync.Map.

## Improvements (should fix)
- [ ] [GetUser] Even once locked, this is a check-then-act race: two
      goroutines can both miss the cache and both call db.LoadUser for the
      same id. Harmless for correctness here but doubles load on a cold key.
      A singleflight wrapper collapses concurrent misses into one load.
- [ ] [GetUser] The cache never evicts and never expires. A stale user record
      is served until restart, and the map grows without bound. Add a TTL or
      a bounded eviction policy.

## Nitpicks (optional)
- [ ] Caching the error case is not done here, which is correct; worth a
      comment so a future edit does not "optimize" by caching failures.

## What's correct
- Cache-miss path correctly propagates the db error instead of caching it.
```

The race is invisible to a single-threaded reading and to passing tests under low concurrency. Classifying as concurrency code is what made "is this read-modify-write safe under goroutines" the first question.

---

## A note on classification

In all three reviews the classification step did the real work. It decided which checklist ran, and the checklist decided which findings rose to Critical. Skip the classification and the same reviewer tends to default to whatever they personally notice first, usually naming and formatting, while the complexity bug, the missing transaction, and the data race go unmentioned.

Classify first. Then the rest of the review aims at the failure modes the code can actually have.

## Further reading

- [`mars-skills`](https://github.com/HermeticOrmus/mars-skills): production-readiness audit for code about to ship
- [`vibe-proof-skills`](https://github.com/HermeticOrmus/vibe-proof-skills): security hardening for full-stack apps
- [`vibe-engineer-skills`](https://github.com/HermeticOrmus/vibe-engineer-skills): the human-side discipline for directing AI codegen
