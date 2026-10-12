# Greek-Inspired Agent Skills

Ten small, composable skills that nudge a coding agent toward disciplined engineering habits: questioning a diagnosis before fixing it, shrinking a bug to its smallest reproduction, backing "it's done" with an actual check, and reaching for a script instead of a framework when a script is enough. Each skill is a single `SKILL.md` — when to use it, a procedure, guardrails, and a stopping condition — so a host loads the full instructions only when that skill is picked.

The names borrow from Greek thinkers as memory hooks. They are modern adaptations, not anything the philosophers wrote about software. (The portraits below are artistic busts and engravings from Wikimedia Commons, not photographs of the people.)

Nothing here is mandatory. A formatting fix needs none of these; a payment refactor might use five.

## The ten patterns

<table>
<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Bust_of_Socrates.JPG?width=240" alt="Bust of Socrates" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Bust_of_Socrates.JPG">Socrates</a></sub>
</td>
<td valign="top">

**`socratic-debugging`** — Question the diagnosis before committing to a fix. Separate what you observed from what you assumed, list the explanations that compete with your first guess, and run the cheapest check that tells them apart.

**Example.** A token refresh keeps failing. Instead of patching the first theory, you line up token expiry, clock skew, overlapping refresh requests, and faulty rotation — then read the timestamps to see which one the evidence actually supports.

</td>
</tr>

<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Aristotle_bust.jpg?width=240" alt="Bust of Aristotle" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Aristotle_bust.jpg">Aristotle</a></sub>
</td>
<td valign="top">

**`aristotelian-reasoning`** — Move from observation to a decision you can defend: collect the facts, classify the problem, map causes to predictions, weigh the options against your constraints, then verify.

**Example.** An endpoint is slow. You measure database time, network delay, serialization, and algorithmic cost separately instead of guessing, optimize the one that dominates, and re-run the same workload to confirm.

</td>
</tr>

<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Portrait_of_Euclid_Wellcome_L0019815.jpg?width=240" alt="Portrait of Euclid" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Portrait_of_Euclid_Wellcome_L0019815.jpg">Euclid</a></sub>
</td>
<td valign="top">

**`euclidean-verification`** — Make "it works" mean something. Turn the claim into observable properties, attach each to a test, inspection, or measurement, and report what you actually checked versus what you are only inferring.

**Example.** Before calling a refactor behavior-preserving, you list the public inputs, error cases, and token lifecycle it must keep, point each at a test or review, and flag anything left unverified.

</td>
</tr>

<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Domenico-Fetti_Archimedes_1620.jpg?width=240" alt="Archimedes, by Domenico Fetti" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Domenico-Fetti_Archimedes_1620.jpg">Archimedes</a></sub>
</td>
<td valign="top">

**`archimedean-decomposition`** — Break a big task into parts with real boundaries and their own acceptance criteria, then spend the effort most people skip: integration.

**Example.** Improving fifty skills at once — inventory them, agree on shared criteria, revise in small batches, validate each, and finish by checking links, naming, and overlap across the whole set.

</td>
</tr>

<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Plato_bust.png?width=240" alt="Bust of Plato" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Plato_bust.png">Plato</a></sub>
</td>
<td valign="top">

**`platonic-quality-review`** — Trade vague praise ("clean", "robust") for criteria you can check. Define what good looks like for this specific artifact, score it point by point, and fix the gaps that matter most.

**Example.** For a teaching skill you define good as accurate, actionable, progressive, and validated, then grade a real skill criterion by criterion instead of on vibes.

</td>
</tr>

<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Pythagoras_Bust_Vatican_Museum_%28cropped%29.jpg?width=240" alt="Bust of Pythagoras" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Pythagoras_Bust_Vatican_Museum_(cropped).jpg">Pythagoras</a></sub>
</td>
<td valign="top">

**`pythagorean-invariant-checking`** — Name the properties that must survive a change, make them testable, and re-check every one afterward — not just whether the code compiles.

**Example.** Refactoring a payment service, you hold the line on correct totals, no duplicate charges, and consistent transaction records, testing both the happy path and the retry path.

</td>
</tr>

<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Bust_of_Heraclitus.jpg?width=240" alt="Bust of Heraclitus" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Bust_of_Heraclitus.jpg">Heraclitus</a></sub>
</td>
<td valign="top">

**`heraclitean-refinement`** — Make each iteration learn. Change one thing for a stated reason, measure against the baseline, and if it did not move the needle, change the hypothesis — not the same code again.

**Example.** A test fails on a race condition. Instead of re-running the same tweak, you revise the diagnosis, add a concurrent-request reproduction, fix the rotation logic, and re-check.

</td>
</tr>

<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Bust_of_Zeno_of_Citium-Ancient_Roman_busts_in_the_Neues_Museum.jpg?width=240" alt="Bust of Zeno of Citium" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Bust_of_Zeno_of_Citium-Ancient_Roman_busts_in_the_Neues_Museum.jpg">Zeno of Citium</a></sub>
</td>
<td valign="top">

**`stoic-control-filter`** — Sort blockers into what you control, what you partly influence, and what you cannot touch here. Act on the first, mitigate the second, and report the third honestly.

**Example.** A remote API is down. You run the local tests and mocks you can, and say plainly that the live integration is still unverified rather than implying it passed.

</td>
</tr>

<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Epicurus_bust.jpg?width=240" alt="Bust of Epicurus" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Epicurus_bust.jpg">Epicurus</a></sub>
</td>
<td valign="top">

**`epicurean-simplicity`** — Pick the simplest option that actually meets the requirements. Every abstraction has to name the need it serves.

**Example.** A one-off documentation transform gets a script and a checklist, not a multi-agent framework — you add orchestration only when scale or reliability demands it.

</td>
</tr>

<tr>
<td width="150" align="center" valign="top">
<img src="https://commons.wikimedia.org/wiki/Special:FilePath/Bust_of_Democritus.jpg?width=240" alt="Bust of Democritus" width="130" /><br/>
<sub><a href="https://commons.wikimedia.org/wiki/File:Bust_of_Democritus.jpg">Democritus</a></sub>
</td>
<td valign="top">

**`democritean-reproduction`** — Shrink a messy failure to the smallest case that still breaks, keeping only the condition that triggers it, so both cause and fix are easy to see.

**Example.** A flaky integration test collapses to two concurrent token-refresh requests with controlled state — the minimal case that reliably trips the race.

</td>
</tr>
</table>

### And the coordinator

**`greek-engineering-workflow`** is not an eleventh technique — it sequences the other ten for a full task and tells you which to skip. A typo fix might use none of them; a security-sensitive refactor might use most.

## Using them together

The examples above keep circling the same auth bug on purpose. Here is one disciplined pass at it, end to end:

1. **Socratic** — don't trust the first explanation ("the access token expired"); list the alternatives, including a concurrency bug.
2. **Democritean** — reduce the failure to two overlapping refresh requests with fixed token state. Now it fails every time, not just sometimes.
3. **Euclidean** — state what the fix must prove ("rotation is safe under concurrent refresh") and decide what evidence counts.
4. **Heraclitean** — fix the rotation, re-run the minimal case and the surrounding suite; if it still fails, revise the diagnosis instead of repeating the change.
5. **Stoic** — if a dependency is unreachable, report what passed locally and what stayed unverified.

Most tasks need only two or three of these. The point is not to run all five — it is to not skip the one that would have caught the bug.

## Where to start

For a day-to-day debugging loop, four skills carry most of the weight:

- **`socratic-debugging`** to challenge the first diagnosis,
- **`democritean-reproduction`** to isolate the smallest failing case,
- **`euclidean-verification`** to tie "done" to real checks, and
- **`heraclitean-refinement`** to make each retry learn and to stop on an objective condition.

Add **`pythagorean-invariant-checking`** for refactors, **`archimedean-decomposition`** for larger work, and **`stoic-control-filter`** when flaky networks, missing permissions, or unavailable services can block verification.

## Install

Add a skill with `npx`:

```bash
npx skill <name>
```

For example, the starter set, one per line:

```bash
npx skill socratic-debugging
npx skill democritean-reproduction
npx skill euclidean-verification
npx skill heraclitean-refinement
```

Valid names are the ten patterns above plus `greek-engineering-workflow`.

Prefer to do it by hand? A skill is just a directory with a `SKILL.md` inside — copy the ones you want into your host's skills location (for Claude Code, a project's `.claude/skills/`), keeping each directory intact.
