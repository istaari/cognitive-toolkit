# Cognitive Toolkit

A structured reasoning skill for software developers and coding agents. It helps choose and apply practical reasoning techniques to debug software, investigate root causes, review code, evaluate designs, and make estimates—while keeping assumptions, evidence, and confidence explicit.

## What it helps with

- **Debugging and root-cause analysis:** reproduce failures, trace incorrect outputs backward, test hypotheses, and use the 5 Whys.
- **Regression and environment issues:** use Git bisection and compare working versus failing environments.
- **Code correctness and safety:** reason about invariants, edge cases, concurrency, and failure modes.
- **Performance investigations:** profile first, identify bottlenecks, and verify improvements with measurements.
- **Design decisions:** compare trade-offs, challenge assumptions, and run a pre-mortem.
- **General problem solving:** apply deductive, inductive, abductive, Bayesian, systems, and decision-analysis techniques.
- **Clear reporting:** distinguish measured or inspected evidence from assumptions, and label conclusions by confidence.

## Repository contents

```text
cognitive-toolkit/
├── SKILL.md
└── references/
    ├── developer-playbooks.md
    ├── developer-techniques.md
    ├── fallacies.md
    ├── techniques.md
    └── worked-examples.md
```

- **`SKILL.md`** — main skill definition, decision guide, workflows, verification rules, and response format.
- **`references/techniques.md`** — general reasoning techniques, with steps, examples, and failure modes.
- **`references/developer-techniques.md`** — software-specific techniques including bisection, differential analysis, minimal reproduction, backward tracing, invariants, failure-mode analysis, performance reasoning, test design, and trade-off analysis.
- **`references/developer-playbooks.md`** — step-by-step playbooks for common engineering problems.
- **`references/worked-examples.md`** — example applications of the methods.
- **`references/fallacies.md`** — logical fallacies, cognitive biases, and common developer traps to check during verification.

## Use the skill

This repository is organized as an agent skill. To use it in a compatible coding-agent environment, make the skill directory available to the agent and follow that environment's instructions for installing or registering skills.

For environments that support a `SKILL.md` convention, the skill metadata identifies it as `reasoning-techniques`. The description recommends using it for debugging, root-cause analysis, flaky tests, performance investigations, code correctness and safety reviews, design choices, estimation, and requests to reason step by step.

> Installation paths and skill discovery behavior vary by agent. Follow your agent's current documentation; this repository does not assume a single vendor-specific installation path.

## Workflow

The skill uses a repeatable process:

1. **Frame the question.** State the problem that needs to be answered.
2. **Gather evidence.** Inspect code, configuration, logs, history, inputs, and versions as appropriate.
3. **Choose a technique.** Select the method that fits the problem rather than applying every technique.
4. **Apply it in order.** Follow the technique's steps and record the reasoning.
5. **Verify.** Check assumptions, search for counterexamples and alternative explanations, and run relevant tests or measurements where possible.
6. **Report the result.** State the conclusion, evidence, confidence, assumptions, and next verification step.

## Core principles

- **Reproduce before fixing.** If a failure cannot be reproduced, say so and gather evidence instead of claiming a verified fix.
- **Inspect before changing.** Prefer read-only checks before actions that could alter or delete data.
- **Test hypotheses.** Make predictions that can be disproved, then test them.
- **Change one thing at a time.** This helps identify what caused a result.
- **Fix causes, not symptoms.** Avoid masking errors without understanding why they occur.
- **Measure performance.** Profile and benchmark before optimizing.
- **Protect secrets.** Do not expose credentials or personal data from logs and traces.
- **Calibrate certainty.** A plausible explanation is not a proof; passing tests is evidence, not a guarantee of correctness.

## Evidence and confidence

The skill distinguishes evidence by origin:

| Tag | Meaning |
|---|---|
| `[ran]` | A test, reproduction, measurement, or query was run. |
| `[read]` | Code, configuration, or history was inspected. |
| `[docs]` | Documentation, release notes, or an issue tracker supports the fact. |
| `[recall]` | Based on remembered general behavior; may not match the current version. |
| `[assumed]` | Unverified assumption. |

Conclusions should use confidence labels—**Certain**, **Probable**, **Plausible**, or **Speculative**—that match the strength of the evidence and state what the conclusion depends on.

## Examples of technique selection

| Situation | Useful technique |
|---|---|
| A regression appeared between two known states | Bisection, then differential analysis |
| A bug has many moving parts | Minimal reproduction |
| The output is wrong but its origin is unclear | Backward tracing |
| State must always obey a rule | Invariant reasoning |
| A network or async operation can fail partially | Failure-mode analysis |
| An application is slow | Profile-first performance reasoning |
| A design has competing constraints | Trade-off analysis and pre-mortem |
| An explanation needs testing | Hypothesis testing |
| A failure needs a lasting fix | Root cause analysis (5 Whys) |

For full procedures and examples, see the files under `references/`.

## Contributing

Contributions that improve clarity, add useful examples, identify missing failure modes, or correct errors are welcome. When proposing a new technique or changing a playbook:

1. Explain the problem the change addresses.
2. Provide steps that another developer can follow.
3. Include a concrete example and common failure modes.
4. Describe how a reader can verify the result.
5. Keep claims about correctness and effectiveness proportional to the evidence.

## License

See [LICENSE](LICENSE) for the license and terms of use.
