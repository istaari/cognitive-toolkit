---
name: archimedean-decomposition
description: "Break complex coding, repository, documentation, migration, or agent-skill tasks into bounded work units with dependencies and acceptance criteria. Use when a task has multiple components or needs staged validation."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.0.0"
---

# Archimedean Decomposition

*A modern engineering adaptation named for Archimedes' habit of splitting a hard problem into tractable parts.*

## Purpose

Make large tasks tractable without losing sight of the integrated outcome.

## Use this skill when

- A task crosses multiple modules, files, APIs, or teams.
- A repository contains many skills, features, tests, or documents to audit.
- Work has dependencies, parallelizable components, or integration risk.
- The task is too large to verify as one undifferentiated change.

For a tiny, independent edit, skip formal decomposition.

## Procedure

1. **Define the outcome.** State the end result and how success will be observed.
2. **Inventory the scope.** Inspect relevant files, modules, interfaces, tests, and constraints before deciding how to split the work.
3. **Choose meaningful boundaries.** Divide by responsibility, dependency, deliverable, or independently testable behavior — not arbitrary file counts.
4. **Define acceptance criteria.** Each work unit needs a concrete result and a way to verify it.
5. **Map dependencies.** Identify prerequisite tasks, shared resources, ordering constraints, and opportunities for safe parallel work.
6. **Prioritize and execute.** Start with high-risk assumptions or blockers. Keep each change focused and reviewable.
7. **Validate components.** Check each unit against its criteria before treating it as complete.
8. **Integrate.** Test cross-component behavior, shared contracts, references, configuration, and end-to-end outcomes.
9. **Reconcile the final state.** Confirm all requested scope is covered and no partial work or inconsistency remains.

## Decomposition rules

- Do not split work so finely that coordination costs exceed the benefit.
- Do not delegate a unit whose acceptance criteria or dependencies are unclear.
- Preserve a single source of truth for interfaces and shared assumptions.
- Reserve effort for integration testing; individually passing parts do not prove the whole system works.
- If discovery changes the scope, update the task map rather than silently continuing with a stale plan.

## Work-unit template

| Field | Description |
|---|---|
| Unit | Short name and bounded responsibility |
| Deliverable | Observable output or change |
| Dependencies | Required prior work or contracts |
| Acceptance criteria | Conditions that must be true |
| Validation | Test, inspection, or measurement |
| Status | Not started / in progress / blocked / verified |

## Example

To improve a large collection of agent skills: inventory the skills, establish shared quality criteria, group similar skills, revise manageable batches, validate each changed skill, then check links, naming, overlap, and consistency across the full collection.

## Completion criteria

- Every requested outcome maps to one or more work units.
- Each unit has clear acceptance criteria and validation.
- Dependencies are respected.
- Integration and end-to-end behavior have been checked, or the remaining integration risk is explicit.
