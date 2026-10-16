# Reasoning Techniques Reference

For software-specific techniques (bisection, differential analysis, minimal reproduction, backward tracing, invariants, failure modes, performance, test design, trade-offs), read `developer-techniques.md`.

## Contents
1. Deductive reasoning
2. Inductive reasoning
3. Abductive reasoning
4. First principles thinking
5. Root cause analysis (5 Whys)
6. Hypothetico-deductive method
7. Probabilistic (Bayesian) reasoning
8. Decomposition
9. Counterfactual reasoning
10. Inversion
11. Fermi estimation
12. Analogical reasoning
13. Process of elimination
14. Pre-mortem
15. Systems thinking
16. Occam's razor
17. Proof by contradiction (reductio ad absurdum)
18. Proof by cases
19. Decision analysis (expected value)

Each section has: **Use when**, **Steps**, **Example**, **Failure modes**.

---

## 1. Deductive reasoning

**Use when:** You have rules or facts that you trust. You need a conclusion that must be true.

**Steps:**
1. Write the general premise (the rule).
2. Write the specific premise (the case).
3. Check that the form is valid (see forms below).
4. State the conclusion.
5. Check that each premise is true. A valid form with a false premise gives an unreliable conclusion.

**Related deductive methods:** See section 17 (proof by contradiction) and section 18 (proof by cases).

**Valid forms:**
- Modus ponens: If P then Q. P. Therefore Q.
- Modus tollens: If P then Q. Not Q. Therefore not P.
- Hypothetical syllogism: If P then Q. If Q then R. Therefore if P then R.
- Disjunctive syllogism: P or Q. Not P. Therefore Q.

**Example:**
Rule: All requests with an expired token return HTTP 401.
Case: This request returned HTTP 200.
Conclusion (modus tollens): The token was not expired.

**Failure modes:**
- Affirming the consequent: "If P then Q. Q. Therefore P." This is invalid.
- Denying the antecedent: "If P then Q. Not P. Therefore not Q." This is invalid.
- A premise is too broad ("all", "never") and has exceptions.

---

## 2. Inductive reasoning

**Use when:** You have many observations. You want a general rule or a prediction.

**Steps:**
1. List the observations. Count them.
2. Check that the sample is large enough and not biased.
3. Find the pattern.
4. State the rule as a probability, not as a certainty.
5. Look for one case that breaks the rule.

**Example:**
The last 40 deployments that changed the config file caused a restart. Rule: a config change probably causes a restart. Confidence: probable.

**Failure modes:**
- Hasty generalization: the sample is too small.
- Selection bias: the sample is not representative.
- Survivorship bias: you only see the cases that remain.
- The "black swan": one new case disproves the rule.

---

## 3. Abductive reasoning

**Use when:** You have a surprising observation. You need the most likely explanation. This is the base of diagnosis and detective work.

**Steps:**
1. Write the observation that needs an explanation.
2. List at least three candidate explanations.
3. For each candidate, ask: if this is true, does the observation follow?
4. Compare candidates on: fit with the facts, simplicity, prior likelihood, and testability.
5. Select the best candidate. Label it plausible or probable.
6. Name one test that can confirm or reject it.

**Example:**
Observation: The service is slow only at 09:00 each day.
Candidates: (a) a scheduled job, (b) a daily traffic peak, (c) a cache that expires at 09:00.
Test: Compare the job schedule and the cache TTL with the time of the slowdown.

**Failure modes:**
- You stop at the first explanation.
- You do not test the explanation, so a guess becomes a "fact".
- Confirmation bias: you only look for evidence that supports your favorite candidate.

---

## 4. First principles thinking

**Use when:** The problem is new. The usual solution is too costly. You doubt the common assumptions.

**Steps:**
1. Write the goal.
2. List the current assumptions about how to reach it.
3. Question each assumption. Ask: "Is this a fact, a rule, or a habit?"
4. Remove the assumptions that are only habits.
5. Find the basic facts that remain (physics, math, cost, constraints, user need).
6. Build a new solution from only those basic facts.
7. Compare the new solution with the current one.

**Example:**
Goal: Lower the cost of a report.
Assumption: "We must buy the report from a vendor." This is a habit. Basic fact: the report needs three data fields and one formula. Result: Build the report from the raw data.

**Failure modes:**
- You discard an assumption that is a real constraint (law, safety, physics).
- You rebuild what already works well. This wastes time.
- You stop at "basic" facts that are not basic.

---

## 5. Root cause analysis (5 Whys)

**Use when:** Something failed. You need to find the cause so that it does not happen again.

**Steps:**
1. Write the problem as a fact. Include time, place, and effect.
2. Ask "Why did this happen?" Write the answer.
3. Ask "Why?" about that answer. Repeat until you reach a cause that you can fix and that is under your control. Five times is a guide, not a rule.
4. For each answer, check that it has evidence.
5. If one "why" has two answers, follow both branches.
6. Write the corrective action for the root cause, not only for the symptom.

**Example:**
Problem: The build failed.
Why? A test timed out. Why? The database was slow. Why? A missing index. Why? The migration did not run. Why? The deploy script skipped it. Root cause: the deploy script skips migrations when a flag is set.

**Failure modes:**
- You stop at a human error ("the engineer made a mistake") and do not ask why the system allowed it.
- You follow only one branch.
- An answer is a guess, not a fact.

---

## 6. Hypothetico-deductive method

**Use when:** You have an explanation and you can test it. This is the method of science and of debugging.

**Steps:**
1. Write the hypothesis in a form that can be false.
2. Deduce a prediction: "If the hypothesis is true, then we will see X."
3. Design a test that can show X or not-X.
4. Run the test.
5. If X does not occur, reject or change the hypothesis. If X occurs, the hypothesis is supported, but not proved.
6. Repeat with a different prediction.

**Example:**
Hypothesis: The memory leak is in the image cache.
Prediction: If we disable the cache, memory use stays flat.
Result: Memory still rises. The hypothesis is rejected.

**Failure modes:**
- The hypothesis cannot be false (it is not testable).
- You change the hypothesis after each failed test so that it always survives.
- You change more than one variable in the test.

---

## 7. Probabilistic (Bayesian) reasoning

**Use when:** The evidence is uncertain. You must update a belief when new data arrives.

**Steps:**
1. State the hypothesis H.
2. Give a prior probability P(H). Use the base rate if you have it.
3. State the evidence E.
4. Estimate how likely E is if H is true: P(E|H).
5. Estimate how likely E is if H is false: P(E|not H).
6. Update: the posterior rises when P(E|H) is much larger than P(E|not H). Use natural frequencies ("out of 1,000 cases") to avoid mistakes.
7. Report the new probability and the range of error.

**Formula:** P(H|E) = P(E|H) x P(H) / P(E)

**Example:**
A disease affects 1 in 1,000 people. A test is 99% accurate in both directions. A person tests positive. The chance of disease is about 9%, not 99%, because the base rate is low.

**Failure modes:**
- Base rate neglect: you ignore the prior.
- You confuse P(E|H) with P(H|E).
- You give a number with false precision. Use ranges when the inputs are guesses.

---

## 8. Decomposition

**Use when:** The problem is too large to solve at one time.

**Steps:**
1. State the whole problem.
2. Split it into parts that do not overlap and that together cover the whole (MECE).
3. Solve each part. Start with the part that has the most risk or the most unknowns.
4. Check the interfaces between the parts.
5. Combine the results.
6. Check that the combined result solves the whole problem.

**Example:**
"Why is revenue down?" -> Revenue = visitors x conversion rate x order value. Check each factor.

**Failure modes:**
- The parts overlap or leave a gap.
- You solve the parts correctly, but the parts interact and the combined result is wrong.

---

## 9. Counterfactual reasoning

**Use when:** You need to know the effect of a cause, or to test whether a factor really matters.

**Steps:**
1. Write the actual event and its outcome.
2. Change one factor: "What if X had not happened?"
3. Follow the consequences of the change.
4. Compare the new outcome with the actual outcome.
5. If the outcome is the same, X was not necessary. If the outcome is different, X had a causal role.

**Example:**
"If we had not changed the pricing, would churn still have risen?" Check the churn of a group that did not see the price change.

**Failure modes:**
- You change more than one factor.
- The change is not realistic.
- Hindsight bias: the outcome looks more certain after it has happened.

---

## 10. Inversion

**Use when:** The goal is hard to reach directly. Failure modes are easy to list.

**Steps:**
1. Write the goal.
2. Invert it: "What would make this fail, or give the worst result?"
3. List the failure causes.
4. Write the opposite of each: these are the things to do or to avoid.
5. Add them to the plan.

**Example:**
Goal: A reliable release process. Inverted: "How can we guarantee a bad release?" Skip tests, deploy on Friday, have no rollback. Avoid each one.

**Failure modes:**
- You list only the obvious failures.
- You avoid every risk and do nothing.

---

## 11. Fermi estimation

**Use when:** You need an order-of-magnitude number and have little data.

**Steps:**
1. State the quantity.
2. Break it into factors that you can estimate.
3. Estimate each factor. Use round numbers.
4. Multiply or add the factors.
5. Do a sanity check with a second method if you can.
6. Report the result as a range or as a power of ten.

**Example:**
Q: How many requests per second for 2 million daily users?
Each user makes about 50 requests/day: 100 million/day. A day has about 86,400 s (about 10^5). Average: about 1,000 req/s. Peak is about 3x the average: about 3,000 req/s.

**Failure modes:**
- A factor is wrong by a large amount, and the error multiplies.
- You report a precise number for a rough estimate.

---

## 12. Analogical reasoning

**Use when:** A similar problem is already solved, or the user needs an explanation by comparison.

**Steps:**
1. Describe the new problem (the target).
2. Find a known problem (the source) with a similar structure.
3. Map the parts of the source to the parts of the target.
4. Carry the solution from the source to the target.
5. List the differences between the two. Check that the differences do not break the solution.

**Example:**
A load balancer is like a traffic controller. It sends cars (requests) to open lanes (servers). The analogy breaks when it is applied to sessions: cars do not need to return to the same lane.

**Failure modes:**
- The similarity is only on the surface.
- You do not check where the analogy fails.

---

## 13. Process of elimination

**Use when:** There is a finite list of options and you have clear constraints.

**Steps:**
1. List all the options.
2. List the constraints or facts.
3. Test each option against each constraint.
4. Remove each option that breaks a constraint. Write the reason.
5. If one option remains, check it against all the constraints again.
6. If more than one option remains, find a new test that separates them.

**Example:**
Constraint table for a logic puzzle. Each clue removes some combinations. The last combination is the answer.

**Failure modes:**
- The list of options is not complete. The real answer is not on the list.
- You remove an option because of a wrong constraint.

---

## 14. Pre-mortem

**Use when:** You have a plan. You want to find its weak points before you start.

**Steps:**
1. Assume that the plan failed in the future.
2. Write why it failed. List as many reasons as you can.
3. Rank the reasons by probability and impact.
4. For each high-ranked reason, write a prevention action and an early warning sign.
5. Update the plan.

**Example:**
"The launch failed." Reasons: the database could not handle the load; the support team was not trained. Actions: run a load test; hold a training session.

**Failure modes:**
- You list only external causes and ignore the team's own weak points.
- You write the list but do not change the plan.

---

## 15. Systems thinking

**Use when:** Many parts affect each other. A change in one place has effects in other places. Past fixes caused new problems.

**Steps:**
1. List the parts and the people or things that act in the system.
2. Draw the links between them.
3. Find the feedback loops. A reinforcing loop grows or shrinks. A balancing loop pushes toward a target.
4. Find delays. A delay can hide the effect of a change.
5. Predict the second-order effects of a change.
6. Choose the change with the most leverage and the fewest harmful side effects.

**Example:**
Add more servers to reduce the load. More servers cause more database connections. The database becomes the new bottleneck.

**Failure modes:**
- The system boundary is too small, so important links are outside it.
- You treat a symptom and cause a new problem.

---

## 16. Occam's razor

**Use when:** Two or more explanations fit the facts equally well. You need a tie-breaker.

**Steps:**
1. Check that the explanations fit the facts equally.
2. Count the assumptions that each explanation needs.
3. Prefer the explanation with fewer unverified assumptions.
4. Mark the result as a working choice. Look for a test that can separate the explanations.

**Example:**
A single failing disk explains all the errors. A coincidence of three separate faults also explains them. Start by checking the disk.

**Failure modes:**
- You use it to reject a more complex explanation that has better evidence.
- You treat "simple" as "true".

---

## 17. Proof by contradiction (reductio ad absurdum)

**Use when:** A direct proof is hard. You can show that the opposite claim leads to an impossible result.

**Steps:**
1. State the claim C that you want to prove.
2. Assume not-C.
3. Follow the logical consequences of not-C.
4. Find a contradiction (a result that breaks a known fact or itself).
5. Conclude that not-C is false, so C is true.
6. Check that every step from not-C was valid. A bad step can cause a false contradiction.

**Example:**
Claim: The two services never write to the same row at the same time. Assume they do. Then the lock log must show two holders for one row. The log shows one holder only. The assumption is false, so the claim holds for the logged period.

**Failure modes:**
- The contradiction comes from an error in a middle step, not from the assumption.
- The "opposite" is not the true logical opposite of the claim.

---

## 18. Proof by cases

**Use when:** The possibilities are few and cover everything. Each possibility can be checked.

**Steps:**
1. List the cases. Check that they cover all possibilities (exhaustive).
2. Check each case separately.
3. Show that the conclusion is true in every case.
4. State the conclusion.

**Example:**
Claim: The function handles all inputs. Cases: empty list, one item, many items, duplicates. Test each one. All pass, so the claim holds for these cases.

**Failure modes:**
- The case list is not complete. A hidden case breaks the claim.
- Too many cases. Use decomposition or a general argument instead.

---

## 19. Decision analysis (expected value)

**Use when:** You must choose between options, and the options have different costs, gains, and probabilities.

**Steps:**
1. List the options. Include "do nothing".
2. For each option, list the possible outcomes.
3. Give each outcome a probability and a value (money, time, or a score).
4. Compute expected value = sum of (probability x value).
5. Check risk: look at the worst case and the chance of it, not only the average.
6. Test the result: change the key numbers and see if the best option changes (sensitivity check).
7. Choose the option. State what facts would change the choice.

**Example:**
Option A: 80% chance to gain 10, 20% chance to lose 5. Expected value = 0.8 x 10 - 0.2 x 5 = 7. Option B: sure gain of 6. A has the higher average. If a loss of 5 is not acceptable, B can still be the better choice.

**Failure modes:**
- The probabilities and values are guesses, and the result looks more exact than it is.
- You use only the average and ignore a rare, severe loss.
- You leave out an option.