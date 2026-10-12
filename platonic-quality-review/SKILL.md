---
name: platonic-quality-review
description: "Evaluate a skill, specification, document, design, or implementation against an explicit quality target and observable rubric. Use for audits, design reviews, documentation improvements, and consistent quality checks."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.0.0"
---

# Platonic Quality Review

*A modern engineering adaptation named for Plato's ideal forms — there is no single ideal for every artifact, so define the target per artifact.*

## Purpose

Translate an abstract ideal of quality into concrete review criteria, compare the current artifact against them, and improve the highest-impact gaps.

## Use this skill when

- Reviewing agent skills, documentation, APIs, designs, or implementations.
- A team uses vague quality terms such as “clean,” “robust,” “beginner-friendly,” or “production-ready.”
- Multiple artifacts need a consistent evaluation rubric.
- An existing artifact should be improved without arbitrary rewrites.

## Procedure

1. **Clarify the purpose and audience.** Identify the user, task, environment, constraints, and expected outcome.
2. **Define the quality target.** Turn terms such as accurate, actionable, maintainable, accessible, safe, or complete into observable criteria relevant to this artifact.
3. **Choose evidence for each criterion.** Examples include tests, runnable examples, task walkthroughs, link checks, usability scenarios, compatibility checks, or review against a specification.
4. **Evaluate the current artifact.** Mark each criterion as met, partially met, unmet, or not applicable. Cite concrete evidence or examples for meaningful findings.
5. **Prioritize gaps.** Rank by user impact, risk, frequency, and effort. Fix correctness and safety failures before cosmetic issues.
6. **Make targeted improvements.** Preserve sound material; avoid rewriting merely to make wording different.
7. **Re-evaluate with the same rubric.** Use the original criteria so improvements can be compared consistently.

## Review rubric template

| Criterion | Observable standard | Evidence | Status | Priority / proposed improvement |
|---|---|---|---|---|
| Accuracy | Claims are supported and current for the task context | Source, test, or code path | Met / partial / unmet / N/A | Action |
| Actionability | A reader or agent can perform the steps | Walkthrough or scenario | Status | Action |
| Completeness | Required cases and constraints are covered | Coverage checklist | Status | Action |
| Maintainability | Structure is clear and changes are localized | Inspection | Status | Action |
| Verification | Success and failure can be distinguished | Tests or observable checks | Status | Action |

Tailor this rubric; do not include irrelevant criteria just to fill a table.

## Decision rules

- Define the target before scoring the artifact.
- Separate evidence-based defects from personal preferences.
- Avoid false precision: numeric scores are useful only when the scale is defined and consistently applied.
- Do not treat an unchecked criterion as satisfied.
- For an agent skill, test realistic tasks and edge cases rather than judging only its prose.

## Example

For an educational coding skill, define standards for technical accuracy, progressive explanations, runnable examples, common failure cases, clear stopping conditions, and validation. Test the skill against representative learner tasks, fix the highest-impact gaps, then repeat the same evaluation.

## Completion criteria

- Quality goals are observable and tied to the artifact’s purpose.
- Findings cite evidence and distinguish severity from preference.
- High-impact gaps are addressed or explicitly left unresolved.
- The same rubric is used for re-evaluation.
