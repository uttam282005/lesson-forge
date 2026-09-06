# Pedagogy notes — domain-specific guidance for CS, math, and software engineering

Read once per curriculum-building session, not per lesson. This file exists because "write a good exercise" means something different in each of these three domains — a generic "practical exercise" prompt produces mediocre results everywhere; a domain-matched exercise shape produces good ones.

## General section guidance (all domains)

**Objectives**: write as actions the learner can do, not topics "covered." Bad: "Understand recursion." Good: "Write a recursive function to traverse a tree and explain why the base case prevents infinite recursion." If you can't picture how you'd test the objective, rewrite it.

**Core content**: depth calibration is the single biggest lever. "Survey" = readable in 5–10 min, prioritizes shape over precision. "Mastery" = mechanism, edge cases, why-not-alternatives. Lead with a concrete example or motivating problem before generalizing, especially below mastery depth. Subheadings every 3–5 paragraphs — one undifferentiated block is a sign to restructure. Name well-known misconceptions explicitly; pre-empting confusion beats hoping the correct version alone sinks in.

**Revision notes**: this is re-read with zero other context, days or weeks later. Ruthless skimmability: short bullets, bolded key terms, no throat-clearing. 3–5 things that matter most, not everything the lesson covered. Isolate anything worth memorizing verbatim (a formula, a syntax pattern) on its own line.

**Review schedule**: default 1/3/7-day spaced intervals are fine — the point is prompting the behavior, not precision-tuning the algorithm.

---

## CS / algorithms / programming

### Read before write: PRIMM

For code-centric lessons, especially anything introducing a new mechanism (not just applying a known one), structure the practical exercise around **PRIMM** (Predict, Run, Investigate, Modify, Make) rather than jumping straight to "write this from scratch":

- **Predict** — show a short program/snippet, ask the learner to predict its output or behavior before running it. Forces engagement with what the code actually does, not pattern-matching to a syntax template.
- **Run** — they run it (or you show the actual output) and check the prediction.
- **Investigate** — trace through it, annotate it, or answer targeted questions about specific lines ("what would happen if line 4 were removed?").
- **Modify** — small-to-larger edits to the given code, changing behavior incrementally. This is where ownership shifts from "not mine" to "partly mine."
- **Make** — a new problem using the same structures, genuinely theirs.

You don't need all five stages in every exercise — for later lessons in a curriculum (learner already has the schema), it's fine to start at Modify or Make. But for foundational lessons, starting at Make (blank-page code writing on a brand-new concept) is a common failure mode: it front-loads cognitive load before the learner has any model of correct usage. Read before write, always, for new mechanisms.

### Parsons problems for early/foundational lessons

When a lesson introduces a new syntactic pattern or algorithm shape, a **Parsons problem** — give the correct code broken into out-of-order blocks/lines, ask the learner to arrange them correctly — is a genuinely research-supported scaffold between "read the worked example" and "write it cold." It reduces extraneous cognitive load versus free-form writing while still requiring the learner to understand structure and ordering.

- Don't add distractor blocks (extra wrong-looking lines meant to trick) — research shows distractors increase cognitive load and lower success rate without improving transfer. Keep it to the actual correct blocks, shuffled.
- Use for foundational/early-curriculum lessons or genuinely new syntactic/algorithmic shapes, not for every exercise in every lesson — it's a scaffold for the hard transition point, not a replacement for real code-writing practice later.
- In the HTML practice file, render as a numbered list of shuffled code blocks (in a `<pre><code>` each) with instructions to determine the order, and the correctly ordered version in the collapsible solution.

### Debugging and code-reading exercises

Two exercise types that are systematically under-used relative to their teaching value, because they don't feel like "writing code" but train skills that matter as much or more:

- **Debug-the-broken-code**: give code with a real, representative bug (off-by-one, wrong operator, misunderstood API, race condition — pick one that maps to a genuine misconception for this topic, not a typo). Ask the learner to find and fix it, explain the hint as "what class of bug is this" rather than pointing at the line.
- **Read-and-explain**: give an unfamiliar snippet (can be from a real, appropriately-licensed open-source project, paraphrased if needed to respect source limits) and ask the learner to explain what it does and why, before ever asking them to write similar code themselves. Reading code is a distinct, undertrained skill from writing it.

### Trace/predict-output exercises

For mechanism-level topics (how something executes under the hood — memory layout, execution order, what a compiler/interpreter does with given input), a trace exercise (given code + input, produce the exact execution trace or output by hand) tests real understanding in a way "explain how X works" prose cannot — it's checkable and it's where hand-wavy understanding gets caught.

### Complexity/resource analysis

For algorithms/data-structure lessons, include Big-O (time/space) analysis as part of the exercise or quiz where relevant — not as a bolted-on afterthought, but tied to a specific implementation the learner just worked with.

---

## Math

### Worked-example-before-independent-proof

The worked-example effect (Sweller) applies to math as strongly as to code, and research on Parsons-style scaffolds extends to proof construction specifically (arranging proof steps in correct order is a validated scaffold for logic/equivalence proofs). For any lesson introducing a new proof technique or problem type:

1. Walk a full worked example in the core content, narrating *why* each step, not just *what* it is.
2. In the practice file, before asking for an independent proof/solution, consider a **proof-ordering exercise** (give the correct proof's steps shuffled, ask the learner to sequence them) as a scaffold for genuinely new techniques — same rationale as Parsons problems for code.
3. Then the independent exercise: a *structurally parallel but distinct* problem (different specific statement, same technique).

### Polya's four-step framework

Bake this into how the practical exercise's hint is written, whether or not you say "Polya" to the user:
1. **Understand the problem** — what's given, what's asked, restate it.
2. **Devise a plan** — what technique/theorem/prior result plausibly applies.
3. **Carry out the plan** — do the work.
4. **Look back** — does the answer make sense, is there a simpler route, does it generalize.

A hint that only nudges step 1 or 2 ("what theorem from this lesson would apply here?") is a real hint; a hint that does step 3 for them is the answer in disguise.

### Other math-specific exercise types

- **Find-the-error**: present a proof/derivation with a planted flaw (a common one for this topic, not an arbitrary typo); ask the learner to locate and explain it. Trains critical evaluation, not just reproduction.
- **Construct-a-counterexample**: for a false or overly-general claim, ask the learner to find a counterexample — tests genuine understanding of the boundary conditions of a theorem.
- **Translate between representations**: algebraic ↔ graphical ↔ verbal ↔ numerical, where applicable — reveals whether understanding is tied to one notation or genuinely general.
- **Compute/apply**: for procedural/computational math, a worked-example-adjacent problem requiring the actual mechanical steps, not just stating the method.

### Notation

Both templates load KaTeX via CDN. Use `\( ... \)` for inline math and `\[ ... \]` or `$$ ... $$` for display equations. Don't approximate notation with plain-text/unicode symbol soup when KaTeX is available — render it properly.

---

## Software engineering (practical / industry)

This is distinct from CS-the-academic-subject: the point is realistic professional practice, and toy exercises ("write a function that reverses a string") undersell it. Prefer exercises shaped like the actual job:

- **Code review**: give a snippet or small diff with a real issue (a bug, a design smell, a missing edge case, a naming/readability problem) and ask the learner to review it as they would a real PR — what would they comment, what would they approve.
- **Design doc / RFC**: for architecture or systems-design lessons, ask the learner to write a short design doc for a given scenario (a few paragraphs: problem, proposed approach, trade-offs, alternatives considered) rather than just describing a diagram back.
- **Write tests for existing code**: given a function/module without tests, ask the learner to write a test suite covering the meaningful cases (happy path, edge cases, failure modes) — tests real understanding of what the code should do, not just how it's written.
- **Refactor**: given working-but-messy or working-but-not-scaling code, ask for a refactor with a stated goal (readability, performance, testability) — forces trade-off reasoning, not just pattern application.
- **Debug a production-like scenario**: a bug report + relevant code/logs, ask the learner to localize and fix it — mirrors real on-call/debugging work more than an isolated broken-function exercise.

Where the curriculum is "mixed" (e.g. a systems-programming topic that's simultaneously CS-conceptual and industry-practical), it's fine and often better to combine: a CS-style trace/debug exercise on the mechanism, with an SWE-style "how would you verify this in a real codebase" follow-up question in the same or next lesson.

---

## Concrete examples of each exercise type (compact, not full lessons)

Abstract descriptions above are easy to flatten into one generic shape by accident. These are minimal, concrete illustrations of what each type actually looks like on the page — imitate the *shape*, not the specific content.

**Parsons problem** (CS, foundational lesson on e.g. a loop pattern):
> "These 5 lines implement linear search, correctly indented but shuffled. Put them in the right order." → five `.parsons-block` divs, each one line of code, in scrambled order. Solution shows them correctly sequenced with a one-line note on why the bounds-check line must come before the comparison.

**Debug-the-broken-code** (CS/SWE):
> "This function is supposed to reverse a singly linked list in place but returns the original list unchanged. Find and fix the bug." → a ~10-line function with one realistic bug (e.g. the `prev`/`next` pointer swap is in the wrong order). Hint: "Trace what happens to `head` after the first iteration — is anything permanently lost?" (narrows without naming the line). Solution: corrected code + one sentence on the class of bug (lost-reference bug from reordering pointer updates).

**Proof-ordering / Parsons-for-proofs** (math, new technique):
> "Here's a proof that √2 is irrational, by contradiction, with the 6 steps shuffled. Order them correctly." → six short numbered statements out of order. Solution: correct order + why step 4 (the parity argument) must follow step 3, not precede it.

**Find-the-error** (math):
> A 4-line "proof" that 1 = 2, with a planted division-by-zero step. Ask which step is invalid and why. Solution names the exact step and the general principle it violates.

**Code review** (SWE):
> A 15-line function plus one sentence of context ("this handles refund requests"), containing one real issue (e.g. no check for a negative refund amount). Ask: "Would you approve this PR? What would you comment?" Solution: the specific comment a reviewer would leave, and why it matters (not just "add validation" but what breaks without it).

**Design doc** (SWE):
> A scenario ("we need to rate-limit this public API endpoint") and a request for a half-page doc: problem, proposed approach, one alternative considered and why rejected. Solution is a model answer at the same length — not exhaustive, illustrating the level of trade-off reasoning expected.

---

## Anti-patterns (all domains)

- **Hidden answer in the hint**: "hint: have you tried using a hash map with keys as X and values as Y and iterating once?" is the solution with extra words. A real hint narrows the search space without doing the step.
- **Decorative code/diagrams**: an example or diagram that doesn't carry structure the prose couldn't — skip it; it teaches skimming.
- **Trivial or out-of-reach exercises**: both fail to teach — see desirable difficulty in `SKILL.md`.
- **False completeness at "mastery" depth**: naming the topic thoroughly while hand-waving the actual mechanism. If you can't verify a specific mechanistic claim, either verify it (search) or scope the lesson down rather than presenting confident hand-waving as mastery-level rigor.
- **One exercise shape for every lesson**: reusing the same generic "write some code" / "prove this" prompt regardless of what the lesson is actually testing is the default failure mode this file exists to prevent — pick the exercise type that matches what this specific objective needs.
- **Slow down on technical claims**: for both math derivations and systems/mechanism explanations, verify each step rather than reconstructing confidently from memory — a fluent wrong explanation is worse than a correctly-scoped uncertain one.
