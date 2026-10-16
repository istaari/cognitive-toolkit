# Fallacies and Biases Checklist

Use this list in the verification step. Read the argument. Ask: "Does any item below apply?"

## Formal fallacies (the form is invalid)

| Fallacy | Pattern | Why it fails |
|---|---|---|
| Affirming the consequent | If P then Q. Q. So P. | Q can have other causes. |
| Denying the antecedent | If P then Q. Not P. So not Q. | Q can be true for other reasons. |
| Undistributed middle | All A are C. All B are C. So all A are B. | Sharing a property does not make two groups equal. |
| Illicit conversion | All A are B. So all B are A. | The reverse is not given. |

## Informal fallacies (the content is the problem)

| Fallacy | Description |
|---|---|
| Ad hominem | You attack the person, not the argument. |
| Straw man | You attack a weaker version of the argument. |
| False dilemma | You give two options when more exist. |
| Slippery slope | You claim that one step will lead to a chain of bad effects, with no evidence. |
| Circular reasoning | The conclusion is already in the premise. |
| Appeal to authority | You say it is true only because an expert said it. |
| Appeal to popularity | You say it is true because many people believe it. |
| Post hoc | A came before B, so A caused B. |
| Correlation as causation | A and B occur together, so A causes B. |
| Hasty generalization | You use too few cases for a general rule. |
| Equivocation | A word changes its meaning inside the argument. |
| Moving the goalposts | You change the proof standard after the evidence is given. |
| Begging the question | You assume what you must prove. |
| Burden shift | You ask the other side to disprove a claim that you did not support. |
| Appeal to ignorance | You say a claim is true because nobody has disproved it. |
| Cherry-picking | You show only the data that supports your view. |
| Composition / division | You say a part has the property of the whole, or the whole has the property of a part. |
| No true Scotsman | You change the definition so that a counter-example does not count. |

## Cognitive biases that damage reasoning

| Bias | Effect | Counter-measure |
|---|---|---|
| Confirmation bias | You look only for support for your view. | Search for evidence against it. |
| Anchoring | The first number or idea controls the result. | Make an estimate before you see the anchor. |
| Availability bias | You judge by examples that you recall easily. | Use data and base rates. |
| Base rate neglect | You ignore how common an event is. | State the prior first. |
| Hindsight bias | The past looks more predictable than it was. | Record predictions before the result. |
| Sunk cost | You continue because of past cost. | Judge only future cost and benefit. |
| Overconfidence | You think your estimate is more exact than it is. | Give a range. |
| Survivorship bias | You see only the successes. | Ask what you do not see. |
| Framing effect | The same facts lead to different choices when worded differently. | Restate the problem in another way. |

## Developer traps

| Trap | What happens | Counter-measure |
|---|---|---|
| Blame the last change | "It broke after deploy X, so X caused it." Time order is not proof. | Revert X or bisect. Find the mechanism. |
| Works on my machine | Your environment is one data point. | Differential analysis. |
| Confirmation in debugging | You read code to confirm your idea and skip evidence against it. | Write a prediction that can fail. Then test it. |
| Anchoring on the top stack frame | The top frame shows where it crashed, not where the fault began. | Backward tracing. |
| Cascade errors | Later errors are often effects of the first error. | Read the log from the first error. |
| Fix the symptom | A null check, a catch block, a retry, or a sleep hides the fault. | Ask why the value was null or why it failed. |
| Passing test = correct | Tests are samples. They give inductive evidence only. | Check invariants and edge cases. |
| Trust comments and docs over code | Comments and docs go out of date. | Read the code. Run it. |
| "It cannot be the library or the compiler" | It is rare, but it can happen. | Rule out your code first. Then make a minimal repro against the library. |
| XY problem | You solve your idea of the fix and not the real problem. | Ask what the real goal is. |
| Premature optimization | You optimize without measurement. | Profile first. |
| Cargo cult | You copy code or patterns that you do not understand. | Explain the purpose of each line. |
| Retry hides flakiness | A retry hides a race or a bug. | Find the cause. Track the flake rate. |
| Observer effect (Heisenbug) | Logging or a debugger changes the timing and hides the bug. | Use low-overhead tracing. Reproduce under stress. |
| Sunk cost in a hypothesis | You keep testing your idea after the evidence rejects it. | Time-box. Keep a list of what is ruled out. |
| Many changes at once | You cannot tell which change helped. | One change per test. |
| Averages hide tails | Mean latency looks fine and p99 is bad. | Use percentiles. |
| Survivorship in logs | You see only what was logged. | Ask what is not logged. |

## Quick audit

1. Is each premise true, and is there evidence for it?
2. Is the form valid?
3. Is there a hidden assumption?
4. Is there another explanation that fits the facts?
5. Is the sample large and unbiased?
6. Does the confidence label match the strength of the evidence?