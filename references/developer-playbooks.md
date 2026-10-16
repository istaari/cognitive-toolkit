# Developer Playbooks

Each playbook gives a technique chain and the steps to follow. The technique details are in `developer-techniques.md` and `techniques.md`.

## Contents
1. Reproducible bug
2. Regression (it worked before)
3. Flaky test or intermittent failure
4. Performance problem
5. Production incident
6. Code review (is this correct and safe?)
7. Design decision
8. Estimate (effort or capacity)
9. Hypothesis log template

Always apply the "Developer rules" in `SKILL.md`.

---

## 1. Reproducible bug

**Chain:** Minimal reproduction -> Backward tracing -> Hypothetico-deductive -> Root cause analysis

1. Read the full error and stack trace. Find the first error.
2. Reproduce it. Write the exact command or steps. Make it a failing test if you can.
3. Reduce it to the smallest failing case.
4. Trace backward from the wrong value to the first wrong step.
5. State the hypothesis as a prediction: "If X is the cause, then changing Y gives Z."
6. Test the prediction. Change one thing at a time.
7. Fix the cause. Run the repro and the related tests.
8. Search the code for the same pattern in other places.

**Report:** Symptom, repro steps, root cause, fix, what you ran to verify, other places checked.

---

## 2. Regression (it worked before)

**Chain:** Bisection -> Differential analysis -> Root cause analysis

1. Confirm a good state and a bad state. Run both yourself.
2. Write a script that gives pass or fail.
3. Run `git bisect` with the script. See `developer-techniques.md`, section 1.
4. Read the guilty commit. Explain the mechanism. Why does this change cause the failure?
5. Check if the commit is the cause or only exposes an older bug.
6. Fix. Add a regression test that fails on the bad commit and passes on the fix.

**Report:** Good commit, bad commit, guilty commit, mechanism, fix, test.

---

## 3. Flaky test or intermittent failure

**Chain:** Inductive (measure) -> Differential analysis -> Abductive -> Hypothetico-deductive

1. Measure the failure rate. Run the test many times (50 to 200) and count the failures. Record seed, time, and test order.
2. Compare failed runs with passed runs: logs, timing, order, machine, parallelism, data.
3. Check the usual causes:
   - Shared state between tests (globals, caches, singletons, database rows, files)
   - Test order, or parallel workers
   - Time, timezone, or date boundaries
   - Random values with no fixed seed
   - Async race (the test uses a sleep and not a wait for a condition)
   - Resource limits (ports, file handles, memory)
   - External services or network
   - Unordered collections (map or set iteration order)
4. For each cause, predict how to force it. Examples: run the test alone and then in the full suite (shared state); fix the seed (randomness); add a delay at the suspected race point; run under CPU stress (timing).
5. Make the test fail on demand. If it fails on demand, you understand the cause.
6. Fix the cause. Do not use a retry or a longer sleep as the fix. They hide the cause.
7. Verify with many runs. If the original failure rate is p, then n clean runs give confidence of 1 - (1 - p)^n. Example: p = 5% and n = 50 give about 92%.

**Report:** Failure rate before, cause, how you forced the failure, fix, failure rate after (with n).

---

## 4. Performance problem

**Chain:** Profile-first measurement -> Decomposition -> Hypothetico-deductive -> Fermi estimation

1. Define the target and the metric (for example p95 latency under 200 ms at 100 requests per second).
2. Measure the current value. Use the same load, data, and machine each time.
3. Find where the time goes: profiler, trace, query plan, flame graph.
4. Rank by share of total time. Work on the biggest share first.
5. Write the hypothesis and the expected gain. Check it with a quick calculation.
6. Make one change. Measure again. Keep it only if the gain is larger than the noise.
7. Stop when the target is met. Do not continue to optimize.

**Report:** Target, before and after numbers (with percentile and load), the biggest cost, the change, the remaining cost.

---

## 5. Production incident

**Chain:** Mitigate first -> Timeline and differential analysis -> Abductive -> Root cause analysis -> Pre-mortem

1. **Stop the damage first.** Use a rollback, a feature flag, a scale-up, or a failover. Cause analysis comes second. Ask the user before a risky action.
2. **Write the timeline.** When did it start? What changed before it: deploys, config, flags, traffic, dependencies, certificate expiry, data growth, scheduled jobs?
3. **Find the scope.** Who and what is affected? Who and what is not affected? The boundary is a strong clue.
4. **List hypotheses.** For each one, name a read-only check (metric, log, trace).
5. **Confirm with evidence.** Do not act on theory alone. A deploy at the same time is a hypothesis, not a proof.
6. **After recovery,** find the root cause and the contributing factors: why was it not caught, why was it slow to find, why was it so large? Write in a blameless style.
7. **Actions:** prevent it, detect it faster, reduce the impact.

**Report:** Impact, timeline, root cause, contributing factors, mitigation, actions with owners.

---

## 6. Code review (is this correct and safe?)

**Chain:** Invariant reasoning -> Proof by cases -> Failure-mode analysis -> Counterexample search

1. State what the change must do. Use the ticket and the tests. If it is unclear, ask.
2. State the invariants and preconditions that the code relies on.
3. Check each path: normal, empty or null, boundary, error, concurrent, large input.
4. Search for a counterexample: an input or a sequence of events that breaks the code.
5. Check what the change does not cover: other callers, other implementations, migrations, config, docs, tests.
6. Check the risks: untrusted input, missing authorization checks, injection, secrets in logs, data loss.
7. Report each finding with: location, what breaks, how to trigger it, suggested fix, confidence. Mark each one as blocking or non-blocking.

**Report:** Findings sorted by severity. Say clearly what you checked and what you did not check.

---

## 7. Design decision

**Chain:** Decomposition -> Trade-off analysis -> Inversion -> Pre-mortem

1. State the decision, the constraints, and the goal.
2. List the options. Include "keep the current solution".
3. Run the trade-off analysis (`developer-techniques.md`, section 9).
4. Invert: "How could this design fail?" Add the fixes to the plan.
5. Run a pre-mortem on the leading option.
6. Write a short decision record:
   - **Context:** the problem and the constraints
   - **Decision:** what you chose
   - **Options considered:** and why you rejected each
   - **Consequences:** good and bad
   - **Revisit when:** the facts that would change the decision

---

## 8. Estimate (effort or capacity)

**Chain:** Decomposition -> Fermi estimation or Analogical reasoning -> Range

**Effort:**
1. Split the work into tasks that are small enough to estimate.
2. Estimate each task as a range (best case, likely, worst case).
3. Compare with similar work that the team did before.
4. Add time for unknowns: integration, review, testing, deployment.
5. Give a range and a confidence level. State what is not included.

**Capacity:**
1. Estimate users, requests per user, and payload size.
2. Compute the average load. Multiply by a peak factor (often 2 to 5).
3. Add headroom (for example 30 to 50%).
4. Check the result with a load test or with real traffic data.

**Report:** The range, the assumptions, the main risks, and what would change the number.

---

## 9. Hypothesis log template

Use this table in a long debugging session. It prevents repeated tests and shows what you ruled out.

| # | Hypothesis | Prediction (if true) | Test | Result | Status |
|---|---|---|---|---|---|
| 1 | Shared cache is stale | Test fails after the coupon test | Run the two tests in order | Fails every time | Supported |
| 2 | Race in the worker | Failure rate rises under CPU stress | Run 100 times with stress | No change | Rejected |

Status values: Open, Supported, Rejected. Update the table after each test.