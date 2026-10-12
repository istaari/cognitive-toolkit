---
name: euclidean-verification
description: "Verify engineering claims against explicit requirements, premises, and observable evidence. Use before claiming a bug is fixed, a refactor is behavior-preserving, a design is correct, or a task is complete."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.0.0"
---

# Euclidean Verification

*A modern engineering adaptation named for the Euclidean proof chain — an LLM explanation is not a formal mathematical proof.*

## Purpose

Require an explicit evidence trail from requirements to conclusions, so a completion claim rests on checks rather than confidence.

## Use this skill when

- You need to claim that a bug is fixed or a task is complete.
- A refactor must preserve existing behavior or interfaces.
- An architecture, algorithm, migration, or technical explanation makes correctness claims.
- The change is high-impact, security-sensitive, or costly to reverse.

## Procedure

1. **Write the claim precisely.** Replace broad phrases like "works correctly" with observable properties.
2. **List premises and requirements.** Include stated constraints, relevant contracts, supported environmental assumptions, and known facts.
3. **Break the claim into obligations.** Identify the intermediate facts that must hold for the claim to be justified.
4. **Map each obligation to evidence.** Use automated tests, static checks, type checks, executable examples, measurements, code inspection, logs, authoritative documentation, or formal methods as appropriate.
5. **Run the checks.** Prefer executable, repeatable evidence over self-assessment. Record the exact checks attempted and their outcomes.
6. **Inspect gaps and counterexamples.** Look for untested branches, boundary conditions, incompatible assumptions, and conclusions that do not follow from the evidence.
7. **Report the strength of the result.** Distinguish verified, partially verified, inferred, and unverified claims.

## Decision rules

- A passing test supports only the behavior that test actually covers.
- A test not run is not a passing test; a test blocked by the environment is not a pass.
- Do not claim formal proof unless formal methods actually establish the required property.
- If a claim cannot be verified, narrow the claim or report the evidence gap.
- For a refactor, check public interfaces, error behavior, side effects, data shape, and relevant performance or security constraints — not just whether the code compiles.

## Evidence ledger

Use this table when multiple claims need verification:

| Claim / requirement | Evidence required | Check performed | Result | Gap |
|---|---|---|---|---|
| Specific observable property | Test, inspection, measurement, or reference | Exact check or command | Pass / fail / blocked / not run | Remaining limitation |

Do not fill the "check performed" or "result" columns with actions that were not actually performed.

## Example

**Claim:** "This refactor preserves the authentication contract."

List the public inputs, outputs, error cases, token-lifecycle behavior, and concurrency guarantees. Map each property to relevant tests or inspections, run the checks, and report any untested behaviors explicitly.

## Completion criteria

- Every material completion claim has relevant supporting evidence.
- Important unsupported steps and test gaps are visible.
- The final report does not claim more than the checks establish.
