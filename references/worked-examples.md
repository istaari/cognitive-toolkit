# Worked Examples

Three full answers for developers. They use the output format from `SKILL.md`. Use them as a pattern, not as a script. The data in the examples is made up.

## Example 1: Flaky test

**User:** "`test_checkout_total` fails in CI about 1 run in 8. It always passes on my machine."

**Question:** Why does `test_checkout_total` fail in about 12% of CI runs?
**Technique:** Differential analysis (+ Abductive reasoning, Hypothetico-deductive method)

**Evidence:**
- [read] CI history: 6 failures in the last 50 runs (12%).
- [read] The CI config runs tests in parallel (`-n 4`) with random order. The local command runs one process in file order.
- [read] `apply_coupon()` stores the active coupon in a module-level variable. Nothing resets it.
- [ran] `test_apply_coupon` then `test_checkout_total` in one process, random order off: fails 5 of 5 times. `test_checkout_total` alone: passes 5 of 5 times.
- [assumed] No other test changes the coupon state.

**Reasoning:**
1. Differences between local and CI: parallel workers, random order. Same code, same data.
2. Candidate causes: (a) shared state between tests, (b) a race between workers, (c) random test data.
3. Cause (c) predicts failures with different values each time. The failure message is always the same: the total is lower than expected. So (c) is unlikely.
4. Cause (a) predicts: if `test_apply_coupon` runs before `test_checkout_total` in the same process, the second test fails every time.
5. Test: run both tests in that order in one process, with random order off. Command: `pytest -p no:randomly tests/test_coupon.py::test_apply_coupon tests/test_checkout.py::test_checkout_total`.
6. Result: it fails 5 of 5 times. Alone, `test_checkout_total` passes 5 of 5 times. The prediction is confirmed, and the failure now happens on demand.

**Conclusion:** Shared coupon state leaks from `test_apply_coupon` into `test_checkout_total`. The failure appears only when the two tests run in the same worker, in that order. Confidence: **Probable** (not yet confirmed by a fix).
**Assumptions:** No other test touches the coupon state. The CI worker assignment is random.
**What would change this conclusion:** A failure when the tests run in separate processes. A failure after the state reset is added.
**Next step:** Reset the coupon state in a fixture (do not add a retry). Run the test 50 times with random order on. If the original rate is 12%, 50 clean runs happen by chance in less than 1 case in 500.

## Example 2: Performance regression

**User:** "`GET /orders` p95 went from 180 ms to 900 ms after the last release. There are 38 commits in that release."

**Question:** Which change caused the p95 rise, and why?
**Technique:** Bisection (+ Profile-first reasoning, Root cause analysis)

**Evidence:**
- [ran] Dashboard: p95 rose from 180 ms to 900 ms at the release time.
- [ran] Benchmark script: 200 requests, median of 5 runs. Threshold: 400 ms.
- [ran] Bisection over 38 commits: 6 steps (2^6 = 64, which is more than 38). Guilty commit found.
- [read] The guilty commit removed the eager loading of `order.items`.
- [ran] Trace of one request: 1 query for orders and 200 queries for items. Each item query takes about 3.5 ms.

**Reasoning:**
1. A good release and a bad release are known. The search space is ordered, so bisection is the fastest method.
2. The benchmark runs 5 times per step and uses the median. This reduces noise. Result: one commit.
3. The commit removed eager loading. Without it, the code runs one query per order row: an N+1 pattern.
4. Cross-check by calculation: 200 queries x 3.5 ms = 700 ms. Add about 200 ms for the rest of the request. Total about 900 ms. This matches the dashboard.
5. Alternative explanations: a slower database (ruled out: the single orders query is still fast in the trace), a larger response (ruled out: payload size is the same).

**Conclusion:** The commit that removed eager loading caused an N+1 query pattern, and this raised the p95. Confidence: **Probable** (it becomes **Certain** when the fix brings p95 back to about 180 ms in the same benchmark).
**Assumptions:** The benchmark data is similar to production data. No other commit in the range changes the latency.
**What would change this conclusion:** The benchmark still shows 900 ms after eager loading is restored.
**Next step:** Restore eager loading, or load all items with one batch query. Re-run the same benchmark. Add a test that asserts the number of queries for this endpoint.

## Example 3: Code review

**User:** "Is this `withdraw` function safe when two requests arrive at the same time?"

```python
def withdraw(account, amount):
    if account.balance >= amount:   # check
        account.balance -= amount   # update
        return True
    return False
```

**Question:** Can `withdraw` break the rule "balance is never negative" under concurrent calls?
**Technique:** Invariant reasoning (+ Failure-mode analysis)

**Evidence:**
- [read] The function checks the balance and then updates it in two separate steps.
- [read] There is no lock, no transaction, and no atomic operation in this function.
- [assumed] `account` is shared between concurrent requests (same row or same object).

**Reasoning:**
1. Invariant: `balance >= 0`. Precondition: `amount > 0`.
2. With one caller, the check guarantees that the update keeps the invariant.
3. With two callers, search for an order of steps that breaks it. Start: balance = 100. Both callers withdraw 80.
4. Caller 1 checks: 100 >= 80, true. Caller 2 checks: 100 >= 80, true. Caller 1 updates: balance = 20. Caller 2 updates: balance = -60.
5. The invariant is broken. The check and the update are not one atomic step.
6. Side finding: the precondition is not checked. A negative `amount` would increase the balance.

**Conclusion:** The function is not safe under concurrent calls. Confidence: **Certain** (a concrete interleaving breaks the invariant), for the case where `account` is shared.
**Assumptions:** The account is shared and no caller holds a lock.
**What would change this conclusion:** A lock that all callers hold, or a database that serializes these updates.
**Next step:** Use one atomic statement: `UPDATE accounts SET balance = balance - :a WHERE id = :id AND balance >= :a`. Check that exactly 1 row changed. Validate `amount > 0`. Add a concurrency test with two parallel withdrawals. Add a `CHECK (balance >= 0)` constraint as a safety net.