---
name: pythagorean-invariant-checking
description: "Identify and preserve critical behavior, data rules, interfaces, security properties, and other invariants during refactors, migrations, optimizations, or code changes."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.1.0"
---

# Pythagorean Invariant Checking

*A modern engineering adaptation named for the Pythagorean tradition of properties that hold invariant under change.*

## Purpose

Protect the properties that must remain true even when implementation details change.

## Use this skill when

- Refactoring or optimizing existing behavior.
- Changing public APIs, data models, schemas, or serialization.
- Migrating data or modifying transaction logic.
- Working on authentication, authorization, payments, concurrency, or other high-risk flows.

## Procedure

1. **Identify invariants.** Inspect requirements, tests, contracts, callers, and existing behavior to find properties that must remain true.
2. **Make each invariant testable.** Express it as an assertion, test, type constraint, database constraint, contract, or explicit inspection checklist.
3. **Establish a baseline.** Run relevant tests or record current behavior before changing the implementation, when feasible.
4. **Make the smallest change that satisfies the request.** Avoid unrelated changes that expand the invariant surface.
5. **Re-check every affected invariant.** Run the relevant tests and inspect changes to data flow, interfaces, side effects, errors, and concurrency behavior.
6. **Investigate violations.** Do not dismiss failing checks as flaky or obsolete without evidence. Fix the regression or explain why a requirement itself must change.
7. **Report coverage.** State which invariants were checked, how, and which remain unverified.

## Common invariant categories

- **API contract:** inputs, outputs, status codes, errors, and compatibility.
- **Data integrity:** uniqueness, referential integrity, ordering, precision, and valid state transitions.
- **Security:** authorization boundaries, secret handling, token lifecycle, and least privilege.
- **Financial correctness:** rounding, totals, idempotency, and duplicate-operation prevention.
- **Reliability:** retry semantics, cancellation, timeout behavior, and recovery.
- **Performance:** latency or resource bounds, but only where a measurable requirement exists.

Use only categories relevant to the task. Do not invent an invariant solely because it appears in this list.

## Decision rules

- **Check state after failure, not only the returned value.** Assert that rejected, invalid, and error paths leave usage counters, queues, caches, and ordering state unchanged — most invariant violations are invisible if you inspect only the happy-path return value.
- Re-run invariant checks after each error path and boundary input, not just the success path.
- Related: use `euclidean-verification` to turn each invariant into runnable evidence and to report which invariants remain unchecked.

## Invariant table

| Invariant | Why it matters | Enforcement / evidence | Result |
|---|---|---|---|
| A property that must remain true | User or system consequence | Test, assertion, constraint, or inspection | Pass / fail / blocked / not checked |

## Example

For a payment-service refactor, verify that totals remain correct, repeated requests cannot create duplicate charges, and transaction records stay consistent. Test both normal cases and failure/retry paths.

## Completion criteria

- Material invariants have been identified from evidence or requirements.
- Relevant checks run after the change.
- Failures and untested properties are clearly reported.
