---
name: greek-engineering-workflow
description: "Coordinate a practical subset of Greek-inspired reasoning patterns for software engineering, debugging, verification, refactoring, and agent-skill improvement. Use when a task needs a disciplined end-to-end workflow; skip unnecessary stages for simple tasks."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.0.0"
---

# Greek Engineering Workflow

*A coordinating guide; the Greek names are memory aids for modern adaptations, not claims that ancient thinkers designed agent workflows.*

## Purpose

Combine composable reasoning patterns into one evidence-driven workflow.

## Operating principle

Select only the techniques needed for the task’s complexity and risk. Do not force every stage onto trivial work, and do not skip verification merely because the solution seems plausible.

## Workflow

1. **Understand the request — Socratic debugging.** State the goal, challenge the initial diagnosis, separate facts from assumptions, and resolve only material ambiguity.
2. **Structure the problem — Aristotelian reasoning.** Classify the problem, identify constraints, and list plausible causes or solution paths.
3. **Decompose — Archimedean decomposition.** For multi-part work, split the task into bounded components with dependencies, deliverables, acceptance criteria, and validation.
4. **Define success — Platonic quality review.** Turn vague goals into observable criteria and decide what evidence would demonstrate them.
5. **Protect behavior — Pythagorean invariant checking.** When changing an existing system, identify the contracts and properties that must remain true.
6. **Isolate failures — Democritean reproduction.** When a bug or test failure is complex, reduce it while preserving the triggering conditions.
7. **Choose a sufficient solution — Epicurean simplicity.** Compare realistic options and avoid unjustified complexity.
8. **Implement and adapt — Heraclitean refinement.** Make a focused change, evaluate it, update the approach from evidence, and observe the iteration budget.
9. **Verify claims — Euclidean verification.** Map important requirements to tests, inspections, measurements, or other appropriate evidence.
10. **Handle blockers and report — Stoic control filter.** Distinguish controllable work from external limitations and report only what was actually verified.

These stages are a menu, not a mandatory linear pipeline. For example, a tiny typo fix may need only the relevant edit and a quick inspection. A security-sensitive refactor may need most stages.

## Technique selection guide

| Situation | Primary technique(s) | Add when needed |
|---|---|---|
| Unclear diagnosis | `socratic-debugging` | `aristotelian-reasoning`, `democritean-reproduction` |
| Broad, multi-module task | `archimedean-decomposition` | `platonic-quality-review`, `euclidean-verification` |
| Refactor / migration | `pythagorean-invariant-checking` | `euclidean-verification`, `heraclitean-refinement` |
| Hard-to-reproduce bug | `democritean-reproduction` | `socratic-debugging`, `heraclitean-refinement` |
| Architecture choice | `epicurean-simplicity` | `aristotelian-reasoning`, `platonic-quality-review` |
| Repeated test-and-fix cycle | `heraclitean-refinement` | `euclidean-verification`, `stoic-control-filter` |
| External service unavailable | `stoic-control-filter` | `euclidean-verification` for local checks |
| Skill or documentation audit | `platonic-quality-review` | `archimedean-decomposition`, `euclidean-verification` |

If companion skills are installed and the host supports skill invocation, invoke the relevant skills. Otherwise, apply their procedures directly; do not claim to have activated separate skills when that has not happened.

## Mandatory guardrails

- Define objective completion criteria before a long or iterative task.
- Prefer executable tests and observable checks over self-assessment.
- Do not repeat the same failed attempt without incorporating new evidence or a materially different diagnostic.
- Inspect the final diff for code changes and review cross-component consistency for broad changes.
- Respect a finite iteration budget.
- Never treat a completion marker, confident explanation, or mock result as proof of live success.
- State what passed, what failed or was blocked, and what remains unverified.

## Final report template

Adapt this to the task; omit sections that add no value.

- **Outcome:** What changed or was decided.
- **Rationale:** The key evidence and reasoning.
- **Validation:** Actual checks performed and their results.
- **Constraints / blockers:** Any unresolved dependency or limitation.
- **Remaining work:** Only concrete unfinished or unverified items.

## Completion criteria

The task is complete only when its stated acceptance criteria have been met and relevant verification supports that conclusion. If a criterion cannot be checked, report the task as partially complete or blocked rather than silently lowering the standard.
