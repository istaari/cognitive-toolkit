---
name: heraclitean-refinement
description: "Improve code, tests, prompts, agent skills, or other artifacts through bounded feedback-driven iterations. Use when evaluation results can guide the next change; stop when objective criteria pass or the iteration budget is exhausted."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.0.0"
---

# Heraclitean Refinement

*A modern engineering adaptation named for Heraclitus' idea that each step enters a changed state.*

## Purpose

Make each iteration respond to new evidence instead of repeating the same attempt. This can run inside a Ralph Loop or another bounded execution loop, but a loop alone does not guarantee learning or correctness.

## Use this skill when

- Writing code and running tests repeatedly.
- Debugging failures whose causes become clearer with feedback.
- Refining an agent skill, prompt, document, or design against measurable criteria.
- Optimizing behavior using benchmark or evaluation results.

## Procedure

1. **Define the target before looping.** Write objective completion criteria, required checks, and a maximum iteration budget.
2. **Establish a baseline.** Capture the current failure, test results, evaluation scores, or quality gaps.
3. **Choose one evidence-based change.** State the current hypothesis and why the change should improve the observed result.
4. **Apply the change.** Keep it focused enough that its effects can be evaluated.
5. **Evaluate.** Run the same relevant checks, plus regression checks where needed. Record outcomes.
6. **Learn from the delta.** Compare results with the baseline and the prediction. If the expected improvement did not occur, revise the hypothesis rather than repeating the same action unchanged.
7. **Continue selectively.** Prioritize remaining blockers, avoid unrelated polishing, and track the iteration count.
8. **Stop deliberately.** Stop when all required criteria pass, the iteration or resource budget is reached, no justified next action remains, or a blocker requires external input.
9. **Report status honestly.** Distinguish completed, partially completed, failed, blocked, and unverified outcomes.

## Loop record

| Iteration | Hypothesis | Change | Check and result | What was learned | Next action |
|---|---|---|---|---|---|
| 1..N | Why this may work | Focused change | Actual evidence | How belief changed | Continue / stop |

## Guardrails

- A completion marker is not evidence that the objective has been met.
- Do not claim that tests passed unless they were actually run and passed.
- Preserve a clear iteration limit; do not continue indefinitely.
- If the same check fails for the same reason, require a changed hypothesis or a meaningful new diagnostic before another iteration.
- Inspect the final diff and run relevant regression checks before declaring a code task complete.
- Do not optimize a metric at the expense of explicit requirements or invariants.

## Example

A coding agent changes a refresh-token path, runs the targeted test, and observes a race-condition failure. Instead of repeating the same change, it updates its diagnosis, adds a concurrent-request reproduction, makes a targeted fix, and reruns targeted and regression tests.

## Completion criteria

- Progress is based on feedback from relevant checks.
- Each repeat attempt incorporates evidence or a materially new hypothesis.
- The stopping condition and iteration budget are respected.
- The final report lists actual checks and remaining limitations.
