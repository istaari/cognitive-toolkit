---
name: stoic-control-filter
description: "Handle blockers and external dependencies by separating what the agent can control, partially influence, or cannot control. Use for unavailable services, permissions, flaky networks, missing tools, or automation requiring honest status reporting."
metadata:
  collection: "greek-inspired-agent-skills"
  version: "2.0.0"
---

# Stoic Control Filter

*A modern engineering adaptation named for the Stoic dichotomy of control.*

## Purpose

Focus effort on actionable factors while making external limits explicit.

## Use this skill when

- A task depends on a remote service, credentials, permissions, network access, or unavailable infrastructure.
- A command or integration fails for environmental reasons.
- A task is only partially verifiable in the current environment.
- You need to report progress without overstating success.

## Procedure

1. **Name the desired outcome.** Identify the exact result that is still needed.
2. **Classify each blocker.**
   - **Controllable:** local code, local tests, documentation, available configuration, and actions within granted permissions.
   - **Partially controllable:** external APIs, dependency compatibility, networks, remote state, and rate limits.
   - **Uncontrollable in this context:** unavailable services, missing authorization that cannot be granted here, unknown future events, or inaccessible systems.
3. **Act on controllable factors.** Make useful local progress and run the checks that are available.
4. **Mitigate partially controllable factors.** Retry only when appropriate, use mocks or fixtures when they answer the relevant question, and distinguish simulated behavior from live integration evidence.
5. **Record the external blocker.** State what is needed to remove it: access, a credential, a service recovery, a user decision, or a later environment.
6. **Define verification boundaries.** Explain which behavior has been checked locally and which remains unverified in the real environment.
7. **Report without exaggeration.** Never infer success merely because a command was attempted or a mock passed.

## Decision rules

- Do not repeatedly retry a stable permission or configuration failure without a reason to expect a different result.
- Do not bypass access controls or invent credentials.
- Mocks can test client behavior under modeled conditions, but they do not prove the live service is working.
- Do not call a task “complete” if an unmet requirement depends on the unresolved blocker. State partial completion precisely.

## Status template

- **Completed:** work and checks that actually succeeded.
- **Blocked:** specific dependency or permission and its observed failure.
- **Verified locally:** checks that passed in the available environment.
- **Not verified:** behavior that requires the unavailable external condition.
- **Next action:** the smallest step needed to unblock or continue.

## Example

If a remote API is unavailable, inspect the client code and run suitable local tests or mocks. Report that local behavior passed, if true, but clearly state that the live integration remains unverified.

## Completion criteria

- Controllable work has been handled where useful.
- External blockers are named specifically, not vaguely.
- Local or simulated evidence is not misrepresented as live verification.
