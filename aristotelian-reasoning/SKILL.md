---
name: aristotelian-reasoning
description: "Turn vague engineering problems into evidence-based decisions by moving from observation and classification to causal hypotheses, solution comparison, action, and outcome verification. Use for complex investigations and technical decisions."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.0.0"
---

# Aristotelian Reasoning

*A modern engineering adaptation named for Aristotle's move from observation to reasoned conclusion.*

## Purpose

Create a clear path from what is observed to what should be done.

## Use this skill when

- The task is broad, technically complex, or easy to misclassify.
- Several system layers could explain the same symptom.
- A technical decision needs a traceable rationale.
- Acting too early could waste time or introduce regressions.

## Procedure

1. **Observe.** Collect relevant facts from the request, repository, logs, metrics, tests, configuration, and documentation. Do not invent missing facts.
2. **Classify.** Identify the type of problem: correctness, performance, reliability, security, usability, compatibility, maintainability, or a combination.
3. **Build causal hypotheses.** Map plausible causes to observable predictions. Distinguish symptoms from root causes and correlation from causation.
4. **Set constraints.** Record acceptance criteria, compatibility requirements, risk tolerance, time or resource limits, and any non-negotiable behavior.
5. **Compare options.** Evaluate the credible options against evidence, expected benefit, risk, cost, reversibility, and complexity.
6. **Choose an action.** Select the best-supported option that satisfies the constraints. Prefer a small reversible experiment when uncertainty is still material.
7. **Verify the outcome.** Compare observed results with the original goal. Run the relevant tests or measurements rather than relying on a persuasive explanation.
8. **Update the model.** If the outcome differs from the prediction, revisit the classification and hypotheses instead of explaining away the mismatch.

## Decision rules

- Use the smallest sufficient amount of investigation for the task’s risk and complexity.
- Do not treat a taxonomy as a diagnosis: classification narrows the search but does not prove a cause.
- Keep requirements and evidence distinct from preferences and assumptions.
- For performance problems, measure before optimizing and compare the same workload before and after.

## Expected output

For complex work, present:

1. Observed facts
2. Problem classification
3. Leading hypotheses and their evidence
4. Constraints and acceptance criteria
5. Chosen action and rationale
6. Verification method and results
7. Remaining uncertainty

For small tasks, the output may be a concise decision with one verification note.

## Example

For a slow API, inspect latency distributions and traces, then test database time, network delay, serialization cost, and algorithmic work separately. Optimize the measured bottleneck and rerun the same representative workload.

## Completion criteria

- The chosen action addresses the classified problem and a supported cause.
- Relevant constraints informed the decision.
- The result was checked against the original goal, or the unverified part is stated clearly.
