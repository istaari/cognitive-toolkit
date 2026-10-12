---
name: epicurean-simplicity
description: "Compare plausible implementations and choose the simplest solution that satisfies explicit requirements, operational constraints, and risk needs. Use for architecture, dependency choices, automation, performance work, and avoiding overengineering."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.0.0"
---

# Epicurean Simplicity

*A modern engineering adaptation named for the Epicurean preference for economy of means.*

## Purpose

Reduce unnecessary complexity while still meeting the real requirements. Simplicity is not the fewest lines of code; it is the least unnecessary cost and complexity without sacrificing correctness, safety, or maintainability.

## Use this skill when

- Choosing an architecture, library, dependency, or agent framework.
- A proposed solution introduces abstractions, services, agents, or infrastructure.
- A small task is at risk of becoming a large system.
- Several options satisfy the functional goal but differ in operational burden.

## Procedure

1. **Define the necessary outcome.** Separate must-have requirements from preferences and speculative future needs.
2. **Set constraints.** Include correctness, security, reliability, performance, compatibility, team familiarity, deployment, and maintenance expectations.
3. **Generate credible alternatives.** Include the current approach and a simpler option when plausible. Do not manufacture weak options just to create a comparison.
4. **Compare total cost.** Weigh implementation effort, runtime cost, dependency risk, operational burden, debugging difficulty, failure modes, migration cost, and future maintenance.
5. **Justify every piece of complexity.** For each abstraction or component, name the requirement or measurable benefit it serves.
6. **Choose the least complex sufficient option.** Prefer reversible changes and proven primitives unless evidence justifies more elaborate machinery.
7. **Validate the choice.** Check the requirements, important edge cases, and any performance or reliability claim used to justify the design.
8. **Record trade-offs.** Explain why the selected option is sufficient and which future condition would justify revisiting it.

## Decision rules

- Do not omit safeguards, tests, or clear interfaces merely to reduce code size.
- Do not add multi-agent coordination, a new dependency, or a service solely because it is available.
- Do not reject complexity that is required by an explicit security, reliability, scale, or compliance need.
- Prefer measured bottleneck fixes over speculative optimization.
- If two options are effectively equivalent, prefer the one that is easier to understand, test, and operate.

## Comparison template

| Option | Requirements met | Complexity / maintenance | Risks | Evidence / unknowns |
|---|---|---|---|---|
| A | ... | ... | ... | ... |
| B | ... | ... | ... | ... |

## Example

For a small documentation transformation, compare a simple script and validation checklist with a multi-agent framework. Prefer the script if it meets the acceptance criteria; add orchestration only when concurrency, specialization, scale, or reliability provides a demonstrable benefit.

## Completion criteria

- The solution meets all explicit requirements and constraints.
- Every material source of added complexity has a justified benefit.
- Key trade-offs and evidence gaps are documented.
