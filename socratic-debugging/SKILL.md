---
name: socratic-debugging
description: "Investigate bugs, ambiguous requirements, unexpected behavior, and weak technical proposals by questioning assumptions, comparing competing hypotheses, and testing the most discriminating evidence before committing to a fix."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.0.0"
---

# Socratic Debugging

*A modern engineering adaptation named for Socratic inquiry — not a method Socrates designed.*

## Purpose

Use disciplined questioning to avoid committing to the first plausible explanation.

## Use this skill when

- A bug has several plausible causes or the initial diagnosis is uncertain.
- Requirements are ambiguous, contradictory, or based on untested assumptions.
- A code review, architecture proposal, or agent skill needs its assumptions challenged.
- A previous fix failed or evidence conflicts with the current explanation.

For a clear, low-risk request with an obvious cause, keep this lightweight or skip it.

## Procedure

1. **State the claim.** Write down the current diagnosis, proposed change, or interpretation of the requirement in one sentence.
2. **Separate facts from assumptions.** Record observed behavior and reproducible evidence separately from guesses, expectations, and missing information.
3. **Expose dependencies.** Ask what must be true for the diagnosis to be correct. Identify implicit assumptions about inputs, state, timing, dependencies, permissions, and environment.
4. **Generate alternatives.** List a small set of plausible competing explanations. Include at least one explanation that would make the initial diagnosis wrong when the evidence warrants it.
5. **Choose a discriminating check.** Prefer a cheap test, log, code-path inspection, reproduction, or authoritative reference that distinguishes between hypotheses.
6. **Update from results.** Eliminate unsupported hypotheses, revise the diagnosis when needed, and run another targeted check only if useful.
7. **Commit proportionately.** Make a change only when evidence supports it. If evidence is inconclusive, state what remains unknown and avoid presenting a guess as a fact.

## Decision rules

- Ask the user a question only when a missing detail materially changes the solution and cannot reasonably be discovered from the available context.
- Do not turn questioning into an endless checklist. Stop when the evidence is sufficient for a safe decision or when the next check is not worth its cost.
- Do not confuse “possible” with “probable,” or a plausible story with a verified root cause.
- When several causes may coexist, investigate them independently rather than forcing a single-cause explanation.

## Expected output

For substantial investigations, summarize:

- **Problem / claim**
- **Observed facts**
- **Assumptions and unknowns**
- **Competing hypotheses**
- **Checks performed and results**
- **Current diagnosis and confidence**
- **Next action or unresolved question**

For simple work, use the reasoning internally and report only the decision and relevant evidence.

## Example

**Claim:** “Refresh-token failures are caused by an expired access token.”

**Alternatives:** refresh-token expiry, clock skew, concurrent refresh requests, incorrect rotation handling, or a server-side rejection. Inspect token timestamps and refresh code, then reproduce concurrent requests if the evidence points toward a race condition. Change the implementation only after a check narrows the cause.

## Completion criteria

- The initial diagnosis has been tested rather than merely repeated.
- Important assumptions are either supported, disproved, or explicitly unresolved.
- The chosen next action follows from the evidence.
