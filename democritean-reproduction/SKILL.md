---
name: democritean-reproduction
description: "Isolate a bug, flaky test, integration failure, or surprising behavior by reducing it to the smallest reliable reproduction, testing a targeted fix, and then running broader regression checks."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.1.0"
---

# Democritean Reproduction

*A modern engineering adaptation named for Democritus' atomism — reduce a failure to its smallest parts.*

## Purpose

Reduce a complicated failure to the smallest case that still demonstrates it, so cause and effect are easier to inspect and a fix is easier to verify.

## Use this skill when

- A failure has many modules, inputs, dependencies, or setup steps.
- A test is flaky or difficult to diagnose.
- An integration fails only under certain timing, state, or input conditions.
- A previous fix addressed the symptom but the root cause remains uncertain.

## Procedure

1. **Capture the original failure.** Record the environment, inputs, steps, expected behavior, observed behavior, logs, and relevant test command. Never expose secrets in a reproduction.
2. **Make it repeatable.** Rerun the failure to determine whether it is deterministic, intermittent, or environment-dependent.
3. **Reduce one dimension at a time.** Remove unrelated inputs, components, setup steps, or dependencies while checking whether the failure persists.
4. **Preserve necessary conditions.** Keep the smallest state, timing, concurrency, configuration, or data condition required to trigger the failure.
5. **Identify the mechanism.** Use the minimal case to test competing explanations and locate the causal behavior, not merely the line where the error surfaces.
6. **Implement a targeted fix.** Make the smallest change that addresses the demonstrated cause while respecting contracts and invariants.
7. **Verify the minimal case.** Confirm it fails before the fix when feasible and passes after.
8. **Run broader regressions.** Execute related unit, integration, and end-to-end checks appropriate to the change.
9. **Document the reproduction.** Preserve steps and relevant context so the failure can be understood and prevented from returning.

## Guardrails

- Do not reduce away the trigger condition just to produce a simpler but irrelevant example.
- For concurrency or timing failures, retain the ordering or timing condition needed to reproduce them.
- When the triggering input is unknown or boundary-related (zero, maximum, exact window/precision edges), reach for property-based or fuzz testing: a generator over the input space is mechanized reduction — it finds a failing case and shrinks it to a minimal one automatically.
- If the failure cannot be reproduced reliably, report that limitation; use logs, traces, or repeated trials without claiming a deterministic reproduction.
- A passing minimal case alone does not establish that the full system is correct.

## Reproduction record

- **Environment and version:**
- **Minimal inputs / state:**
- **Steps to reproduce:**
- **Expected result:**
- **Actual result:**
- **Repeatability:**
- **Suspected mechanism and evidence:**
- **Fix validated by:**
- **Regression checks:**

## Example

An authentication integration test fails only when two refresh requests overlap. Reduce the case to two concurrent requests with controlled token state, verify it triggers the failure, correct the rotation or synchronization logic, then run the surrounding authentication test suite.

## Completion criteria

- The smallest known reproduction still demonstrates the original failure, or reproducibility limits are documented.
- The fix addresses the supported cause.
- The minimal case and relevant broader regressions have been checked, with results reported accurately.
