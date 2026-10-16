# Developer Techniques Reference

These techniques are for software problems. For general techniques (deductive, abductive, first principles, and others), read `techniques.md`.

## Contents
1. Bisection
2. Differential analysis
3. Minimal reproduction
4. Backward tracing
5. Invariant reasoning
6. Failure-mode analysis (async and distributed systems)
7. Profile-first performance reasoning
8. Boundary and equivalence partitioning (test design)
9. Trade-off analysis (design decisions)

Each section has: **Use when**, **Steps**, **Example**, **Failure modes**.

---

## 1. Bisection

**Use when:** Something worked before and fails now. Or a large input, config, or code path fails. The search space has an order (commits, versions, input lines, config lines).

**Steps:**
1. Find a good state and a bad state. Check both yourself.
2. Write a script that returns pass or fail with no manual judgment.
3. Test the middle point.
4. Keep the half where the state changes from good to bad.
5. Repeat until one commit, line, or item remains.
6. Read the guilty change. Explain why it causes the failure. Do not stop at "it is this commit".

**Tools:**
```bash
git bisect start
git bisect bad                 # current commit is bad
git bisect good <known-good>   # an older commit that works
git bisect run ./check.sh      # exit 0 = good, 1-127 (not 125) = bad, 125 = skip
git bisect reset               # when finished
```
For inputs, halve the input file. For config, disable half of the settings. For code, stub half of the path.

**Example:**
38 commits between two releases. Bisection needs at most 6 tests (2^6 = 64, which is more than 38).

**Failure modes:**
- The check is flaky, so it gives a wrong verdict. Run it several times per step.
- Some commits do not build. Use skip (exit 125).
- The "good" starting point was not really good.
- Two changes together cause the bug. Bisection finds the first bad change. Check for a second one.

---

## 2. Differential analysis

**Use when:** It works in one place and fails in another. Examples: local vs CI, staging vs production, user A vs user B, version 1 vs version 2.

**Steps:**
1. Write the working case (W) and the failing case (F) side by side.
2. List every difference. Check these groups: code version, dependencies, config, environment variables, OS and CPU, data, time and timezone, locale, permissions, network, load, memory and disk, order of operations, leftover state (cache, files).
3. Rank the differences by how likely they are to cause the symptom and how cheap they are to test.
4. Change one difference at a time. Make F like W, or W like F.
5. The difference that flips the result is the cause, or part of it.
6. Ask why that difference matters. Write the mechanism.

**Example:**
A test passes locally and fails in CI. Differences: timezone (local vs UTC), CPU count, file order. Set `TZ=UTC` locally. The test now fails. The date logic assumed the local timezone.

**Failure modes:**
- A hidden difference is not on your list (for example a cache or a leftover file).
- Several differences are needed together.
- You change many differences at once.

---

## 3. Minimal reproduction

**Use when:** The failing case is large, slow, or has many parts. You need a small case to find the cause or to ask for help.

**Steps:**
1. Make the failure repeatable. Fix the seed. Pin versions. Save the input.
2. Remove one part (code, input, config, dependency, or step).
3. If it still fails, keep the removal. If not, put the part back.
4. Repeat until every remaining part is needed.
5. Check that the small case fails for the same reason (same error), not a different one.
6. Keep it as the regression test for the fix.

**Example:**
A 2,000-line input file crashes the parser. After reduction, 3 lines remain. One of them has a trailing space.

**Failure modes:**
- The reduction changes the failure. You found a different bug.
- The bug disappears when you simplify. Timing or memory is involved. The removed part may matter.
- You spend too long on reduction when the cause is already clear.

---

## 4. Backward tracing

**Use when:** You see a wrong value, wrong output, or bad state. You know what the correct result looks like.

**Steps:**
1. Find the first place where the wrong value is visible.
2. Ask: "Where did this value come from?" List the sources (arguments, fields, reads, calls).
3. Check each source with a log, a debugger, or an assertion.
4. Move to the source that is wrong. Repeat.
5. Stop at the first step where the inputs are right and the output is wrong. That step is faulty.
6. Explain why that step gives the wrong value.

**Example:**
The page shows a total of 0. The API returned 0. The query returned no rows. The filter used `created_at > now`, and `now` was in the wrong timezone. The fault is in the filter.

**Failure modes:**
- You assume a layer is correct and do not check it.
- You stop at the symptom (add a null check) and not at the origin.
- You trace a different value that looks similar.
- The top stack frame shows where the program crashed, not where the fault began.

---

## 5. Invariant reasoning

**Use when:** You must show that code is correct. Or you must find why a state becomes invalid. This applies to loops, state machines, data models, and concurrent code.

**Steps:**
1. Write the invariant: a statement that must be true at a given point. Examples: "balance >= 0", "the list is sorted", "the lock is held when the counter changes".
2. Check that it holds at the start (initialization).
3. For each operation that changes the state, assume the invariant holds before it. Show that it holds after it (preservation).
4. For a loop: at exit, the invariant and the exit condition must together give the wanted result.
5. For concurrent code: check every order in which operations on the same state can interleave. Or show that a lock or an atomic operation prevents it.
6. Check the error paths, early returns, and exceptions. A failure halfway through an update can leave a broken state.
7. If a step breaks the invariant, you found the bug or a missing guard.

**Example:**
Binary search. Invariant: if the target exists, it is in `[lo, hi]`. It holds at the start. Each step keeps it true. At exit, `lo > hi`, so the range is empty and the target is absent. Also check value range: in fixed-width integers, `(lo + hi) / 2` can overflow. Use `lo + (hi - lo) / 2`.

**Failure modes:**
- The invariant is too weak to prove the result.
- You check only the normal path.
- You forget partial failure (exception after one of two writes).
- You forget concurrency.

---

## 6. Failure-mode analysis (async and distributed systems)

**Use when:** Code calls a network, a queue, a disk, or another service. Or code runs concurrently.

**Steps:**
1. List **every** external interaction separately: each queue operation, each cache read and write, each provider or API call, each disk write, each lock. Run step 2 against **each** one, not once for the handler as a whole. Build a small matrix (interaction x question). Most missed cases are interactions you never put on the list: you check the queue and the cache and forget the payment call.
2. For each one, ask:
   - What if it fails?
   - What if it times out?
   - What if it succeeds but the reply is lost?
   - What if it runs twice?
   - What if it runs late or out of order?
   - What if it runs only partway?
3. Write the state of the system in each case.
4. Check that the code handles each case: timeout, retry, idempotency key, transaction, compensation, alert.
5. Rank the unhandled cases by likelihood and damage.
6. Fix the top cases. Add a test that forces the case (fault injection).

**Key principle - external side effects:** You cannot atomically both perform an external side effect (charge a card, send an email, publish a message) and record that you performed it. A crash can land between the two steps, and at-least-once delivery then retries. A lock does not fix this: a lock serializes concurrent callers but does nothing across a crash boundary, so a worker that crashes after the charge and before the record will charge again on redelivery. Push idempotency to the provider (send an idempotency key so the provider de-duplicates), make the effect naturally idempotent, or use a durable state machine (`claimed -> charged -> done`) with reconciliation. Treat an atomic claim (one `SET NX`, or `SADD` by return value) as an optimization that avoids wasted concurrent work, not as the correctness guarantee.

**Example:**
A payment call times out. Did the charge happen? You do not know. A retry without an idempotency key can charge the customer twice.

**Failure modes:**
- You list too few cases. For each interaction, check: concurrent callers, a crash at each step, duplicate or redelivery, lost durable state (a cache or replica that forgets), an unstable or missing idempotency key, and bad input.
- You assume the network is reliable.
- You assume exactly-once delivery. Most systems give at-least-once.
- You assume that clocks on different machines agree.
- You test only the happy path.

---

## 7. Profile-first performance reasoning

**Use when:** Code or a system is slower than needed.

**Steps:**
1. Define the target (for example "p95 under 200 ms"). Measure the current value with the same load every time.
2. Measure where the time goes. Use a profiler, a trace, a query plan, or a flame graph. Do not guess.
3. Rank the parts by share of total time. A part that takes 10% of the time can give at most 10% gain.
4. Form a hypothesis for the top item. Common causes: slow algorithm, N+1 queries, missing index, too many allocations, lock contention, slow external call.
5. Estimate the expected gain before you change code (Fermi estimate).
6. Change one thing. Measure again under the same conditions.
7. Keep the change only if the gain is larger than the noise. Repeat until the target is met.

**Example:**
An endpoint takes 900 ms. The trace shows 700 ms in the database: 200 queries at 3.5 ms each (N+1 pattern). Batch them into one query. Expected result: about 200 ms for the rest plus a few ms for the query.

**Failure modes:**
- You optimize without measuring.
- The benchmark does not match production load or data size.
- You measure once. Noise looks like a gain.
- The average hides the tail. Use percentiles (p95, p99).
- You read the median, not the tail. A change can help the common case and hurt the rare one. Classic case: a new index speeds the typical lookup (p50) but the optimizer then picks an index scan with many random heap fetches for a skewed "whale" row, so p99 gets worse. Capture plans and timings for the p99 samples, not the median.
- Warm and cold caches give different numbers.

---

## 8. Boundary and equivalence partitioning (test design)

**Use when:** You must choose which test cases show that code works. Or you must judge whether the tests are enough.

**Steps:**
1. List the inputs and their ranges or states.
2. Split each input into classes where the code should behave the same (valid, invalid, empty, null).
3. Pick one value from each class.
4. Add boundary values: just below, at, and just above each boundary. Also 0, 1, max, max + 1, empty, one item.
5. Add combinations of inputs that interact. If there are too many, use pairwise combinations.
6. Add error cases and failure cases.
7. For each test, write which bug it would catch. If you cannot name a bug, remove the test.

**Example:**
An age field accepts 18 to 65. Test: 17, 18, 19, 64, 65, 66, a non-number, and an empty value.

**Failure modes:**
- You test only the happy path.
- A test passes for the wrong reason (it asserts nothing).
- You test how the code works inside, not what it must do.
- You treat line coverage as proof. Coverage shows which lines ran. It does not show that the results were checked.

---

## 9. Trade-off analysis (design decisions)

**Use when:** You must choose between designs, libraries, or architectures.

**Steps:**
1. State the decision and the fixed constraints (deadline, team skills, budget, compliance, existing systems).
2. List the real options. Include "keep the current solution".
3. List the criteria that matter. Examples: performance, cost, complexity, operability, security, reversibility, time to deliver, team skill.
4. Weight the criteria for this project. Write why.
5. Score each option. Tag the evidence for each score (`[ran]` benchmark, `[docs]`, `[assumed]`).
6. Check reversibility. If you are unsure, prefer the option that is easy to undo.
7. Run a pre-mortem on the leading option: "It failed in one year. Why?"
8. Write the decision, the reasons, and the conditions that would change it (an ADR).

**Example:**
Managed queue vs self-hosted queue. Self-hosted gives lower unit cost. Managed gives lower operating work. The team has no on-call capacity, so operability has the highest weight. Choose managed. Revisit if cost per month passes the agreed limit.

**Failure modes:**
- You choose by popularity.
- All criteria have equal weight.
- You ignore operating cost (who fixes it at 3 a.m.) and migration cost.
- You spend days on a decision that is easy to reverse.