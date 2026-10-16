---
name: reasoning-techniques
description: Structured reasoning for software developers and coding agents. Use this skill whenever the user must debug a bug, find a root cause, explain a failing or flaky test, trace a regression, analyze a performance problem or production incident, check if code is correct or safe, review a change, choose between designs or libraries, estimate effort or capacity, or "think step by step", even if no technique is named. Covers deductive, inductive, abductive, first principles, root cause (5 Whys), hypothesis testing, Bayesian, bisection, differential analysis, minimal reproduction, invariants, failure-mode analysis, trade-off analysis, Fermi estimation and pre-mortem.
---

# Reasoning Techniques for Developers

This skill helps you pick the correct reasoning technique for a software problem. It also helps you apply the technique in clear steps and check the result.

A named technique gives a fixed set of steps. The steps make your reasoning visible. The developer can then find and correct a wrong step.

## When to use this skill

Use it when the answer needs more than one step of thought and a wrong step would waste time or cause damage. Examples: debugging, root cause analysis, regression hunting, performance analysis, incident response, code correctness checks, design decisions, and estimates.

Do not use the full workflow for a syntax question, a command flag, an obvious one-line fix, or a casual question. Answer those directly.

If a task-specific skill is available (for example debug, code review, or architecture decision), follow its workflow. Use this skill for the reasoning method inside that workflow.

## Developer rules

Apply these rules to all code work. They prevent the most common reasoning errors.

1. **Reproduce first.** If you cannot reproduce a problem, you cannot know that a fix works. If you cannot reproduce it, say so. Then collect evidence (logs, traces, inputs) instead.
2. **Read the real error.** Read the full message and the full stack trace. Find the first error. Later errors are often effects.
3. **Predict before you run.** Write what you expect to see. A result that surprises you is the most useful result.
4. **Change one thing at a time.** If you change two things, you cannot know which one had the effect.
5. **Prefer read-only checks.** Read logs, code, and history first. Ask before you run a command that deletes data, changes a database, or touches production.
6. **Fix the cause, not the symptom.** A null check, a catch block, a retry, or a longer sleep can hide the fault. Ask why the value was null.
7. **Verify the fix.** Run the repro and the related tests again. Say what you ran. Do not say "fixed" if you did not run it.
8. **Time-box each hypothesis.** After two or three failed tests, go back to the observations. List what you have ruled out. Choose a new hypothesis.
9. **Protect secrets.** Do not copy tokens, keys, or personal data from logs into your answer.
10. **Compute the number that decides it.** If the conclusion depends on a probability, a rate, a size, or a threshold, calculate it before you conclude. A guessed magnitude is not evidence. Example: with a 2% failure rate, 100 green runs is `0.98^100` ~ 13% likely by chance, so a green streak that length does not prove a fix.

## Evidence levels

Tag each important fact with its source. Higher levels are stronger.

| Tag | Meaning |
|---|---|
| `[ran]` | You ran it or measured it (test, repro, profiler, query, log from the real system). |
| `[read]` | You read the code, config, or history yourself. |
| `[docs]` | Documentation, release notes, or an issue tracker says it. |
| `[recall]` | You remember how it usually works. It may not match this version. |
| `[assumed]` | A guess that you did not check. |

Do not build a certain conclusion on `[recall]` or `[assumed]` facts. Check them first, or lower the confidence.

## Workflow

1. **State the question.** Write the question in one sentence. If the question is unclear, ask one short question to make it clear.
2. **Check the inputs.** List the facts that you have: error text, code, versions, config, logs, what changed. If you need a fact that is missing, get it with a tool (file read, grep, git log, test run, search). If you have no tool, ask the user one short question. If the user cannot answer, state your assumption and continue.
3. **Classify the problem.** Use the selection table below.
4. **Pick one main technique.** Add up to two supporting techniques. More techniques make the answer long and unclear.
5. **Apply the steps.** Read the technique in `references/developer-techniques.md` (software) or `references/techniques.md` (general). For a full procedure, read the matching playbook in `references/developer-playbooks.md`. Write each step in order.
6. **Verify.** Do the checks in the "Verification" section.
7. **Report.** Use the output format below.

## Selection table

### Software problems

| Situation | Main technique |
|---|---|
| It worked before. It fails now. The range of changes is ordered. | Bisection |
| It works in one place and fails in another (local vs CI, staging vs prod). | Differential analysis |
| The failing case is large, slow, or has many parts. | Minimal reproduction |
| A value or output is wrong. You must find where it went wrong. | Backward tracing |
| You must show that code is correct, or why state becomes invalid. | Invariant reasoning |
| Code uses networks, queues, disks, or concurrency. | Failure-mode analysis |
| Code or a system is too slow. | Profile-first performance reasoning |
| You must decide which tests are enough. | Boundary and equivalence partitioning |
| You must choose between designs, libraries, or architectures. | Trade-off analysis |

### General problems

| Situation | Main technique |
|---|---|
| You have rules or facts. You need a certain conclusion. | Deductive reasoning |
| You have many examples. You need a general rule. | Inductive reasoning |
| You have an observation. You need the best explanation. | Abductive reasoning |
| The problem is new, or the usual assumptions are in doubt. | First principles thinking |
| Something failed. You need the cause. | Root cause analysis (5 Whys) |
| You have an explanation. You need to test it. | Hypothetico-deductive method |
| The evidence is uncertain or incomplete. | Probabilistic (Bayesian) reasoning |
| The problem is large. | Decomposition |
| You need to know what would change the result. | Counterfactual reasoning |
| The goal is hard to reach directly. | Inversion |
| You need a number and have little data. | Fermi estimation |
| A similar problem is already solved. | Analogical reasoning |
| You have many options and clear constraints. | Process of elimination |
| A plan could fail. | Pre-mortem |
| Many parts affect each other. | Systems thinking |
| You can assume the opposite and look for a contradiction. | Proof by contradiction (reductio) |
| The cases are few and you can check each one. | Proof by cases |
| Options have different costs, gains, and probabilities. | Decision analysis (expected value) |
| Two explanations fit the facts equally. | Occam's razor (tie-breaker only) |

If two rows fit, use both in order (see "Combining techniques"). Choose by the inputs that you have, not by certainty alone. Deduction gives the most certain result, but it needs rules that you trust. Without trusted rules, use induction or abduction.

## The three core modes

These three modes are the base of most other techniques. Know the difference.

- **Deductive:** General rule + specific case -> certain conclusion. If the premises are true and the form is valid, the conclusion must be true. Example: "Every request with an expired token returns 401. This one returned 200. The token was not expired."
- **Inductive:** Specific cases -> general rule. The conclusion is probable. New data can disprove it. Example: "All 40 test runs passed, so the code is probably correct." A test run is a sample, not a proof.
- **Abductive:** Surprising observation -> best explanation. The conclusion is plausible. It is a hypothesis, not a proof. Example: "Latency rises at 09:00 each day, so a scheduled job is a likely cause."

Most debugging is a loop: abduction (guess the cause) -> deduction (predict what you will see) -> test -> induction (does the pattern hold?). Do not present an inductive or abductive result as a proof.

## Combining techniques

Real problems need more than one technique. Use these chains. Full steps are in `references/developer-playbooks.md`.

- **Reproducible bug:** Minimal reproduction -> Backward tracing -> Hypothetico-deductive -> Root cause analysis.
- **Regression:** Bisection -> Differential analysis on the guilty change -> Root cause analysis.
- **Flaky test:** Inductive (measure the failure rate) -> Differential analysis (failed vs passed runs) -> Abductive -> Hypothetico-deductive (force the failure).
- **Performance problem:** Profile-first measurement -> Decomposition -> Hypothesis test -> Fermi cross-check.
- **Production incident:** Mitigate first -> Timeline and differential analysis -> Abductive -> Root cause analysis -> Pre-mortem for prevention.
- **Code review:** Invariant reasoning -> Proof by cases -> Failure-mode analysis -> Counterexample search.
- **Design decision:** Decomposition -> Trade-off analysis -> Inversion -> Pre-mortem.
- **Estimate:** Decomposition -> Fermi estimation or Analogical reasoning -> Range with stated assumptions.

## Scale the answer to the question

- **Simple question:** Give the conclusion, the key reason, and the confidence. Use 2 to 4 sentences.
- **Normal question:** Use the full output format.
- **Important or risky change** (production, data, security, money): Use the full output format. Run the fallacy audit and the developer traps in `references/fallacies.md`.

Name the technique only when it helps the developer to follow the logic.

## Confidence labels

Use one label for each conclusion:

| Label | General meaning | In code work |
|---|---|---|
| **Certain** | The argument is valid and all premises are verified. | You reproduced it, fixed it, and a test or run now confirms it. Or you found a concrete counterexample. |
| **Probable** | Strong evidence supports it. Counter-evidence is weak or absent. | Logs and code match the symptom. You did not reproduce or confirm it yet. |
| **Plausible** | It fits the facts. Other explanations also fit. | It matches the symptom. Other causes remain open. |
| **Speculative** | It is a guess. It has little support. | No direct evidence. |

Always say what the label depends on. Example: "Probable, if the logs are complete."

## Verification

Do these checks before you report a conclusion:

1. **List the assumptions.** Write each assumption that the conclusion needs. Mark each one as verified or unverified.
2. **Test a counter-case.** Try to find one case where the conclusion is false. If you find one, change the conclusion.
3. **Check for alternatives.** For abductive and causal reasoning, write at least two other explanations. Say why you reject them.
4. **Check for fallacies and traps.** Read `references/fallacies.md` if the change is important or the user asks for a rigorous check.
5. **Check the arithmetic and the logic form.** Do not trust a calculation that you did not check twice.
6. **Use tools to verify.** Run the code, read the file, query the log, or search for the fact. A checked fact is better than a reasoned guess.
7. **Check for the same fault elsewhere.** If you found a root cause, search the code for the same pattern.

## Output format

Use this structure for a full answer. For a simple question, give a shorter answer that keeps the same order of ideas.

```
**Question:** <one sentence>
**Technique:** <main technique> (+ <supporting technique>, if used)

**Evidence:**
- [ran] <fact>
- [read] <fact>
- [assumed] <fact>

**Reasoning:**
1. <step>
2. <step>
3. <step>

**Conclusion:** <answer> - Confidence: <label> (<what it depends on>)
**Assumptions:** <list>
**What would change this conclusion:** <list>
**Next step:** <one concrete action or test>
```

For a long debugging session, keep a hypothesis log. The template is in `references/developer-playbooks.md`.

## Writing rules for reasoning

- Write short sentences. Use one idea in each sentence.
- Show every step that the conclusion needs. Do not skip a step because it looks obvious.
- Separate facts from assumptions. Label each one with an evidence tag.
- Use plain words. If you must use a technical term, define it once.
- Put commands, paths, and code in code blocks.
- If the user wants a different format or language level, follow the user.

## Common mistakes

- **Validity and truth are different.** A valid argument with a false premise can have a false conclusion. Check the premises.
- **Correlation is not causation.** "It broke after the deploy" is a hypothesis. Look for a mechanism, a time order, and a control case.
- **A passing test is a sample.** It is inductive evidence. It does not prove that the code is correct.
- **One cause is rarely the whole cause.** In root cause analysis, look for contributing causes.
- **The first explanation is not always the best.** Write at least two before you choose.
- **Do not use Occam's razor as proof.** It is a tie-breaker. It does not show that the simple explanation is true.
- **Do not hide uncertainty.** A clear "plausible" is more useful than a false "certain".

## Reference files

- `references/developer-techniques.md` - Steps, a short example, and failure modes for nine software techniques. Read the section for the technique you picked.
- `references/developer-playbooks.md` - Step-by-step procedures for bugs, regressions, flaky tests, performance, incidents, code review, design decisions, and estimates. Includes a hypothesis log template.
- `references/techniques.md` - Steps, a short example, and failure modes for 19 general techniques.
- `references/fallacies.md` - Logical fallacies, cognitive biases, and developer traps. Read it for the verification step on important changes.
- `references/worked-examples.md` - Three full developer examples in the output format. Read it if you are not sure how a good answer looks.
