---
name: lesson-forge
description: |
  Builds a personalized curriculum of linked HTML lessons for learning CS, math, and software engineering — domain-specific, not generic "explain a topic" tutoring.

  Trigger for: "teach me X", "help me learn X", "make me a course on X", "I want to get good at X", "build me a curriculum for X" — for CS/programming, math, or software-engineering topics. Always interviews the user on domain, depth, style, and goal before generating anything.

  Each lesson ships as linked HTML files: content (objectives, core content, reading) + practice (exercise, quiz, revision notes, spaced-repetition schedule). Programming exercises get real runnable starter/solution source files, not just code on a webpage. Math renders proper notation (KaTeX). Exercises follow domain-matched structures — PRIMM/Parsons for programming, Polya/worked-example proofs for math, review/design-doc tasks for software engineering.

  Do NOT use for a one-off question ("what is a hash map") — only for a real multi-lesson curriculum.
---

# Lesson Forge

Turns "I want to learn X" into an actual curriculum for CS, math, or software engineering: an ordered set of HTML lesson files the user opens, works through, and revisits. Not a one-shot explainer — a structured course, and the exercise/content design is specific to what actually works for these three domains, not a generic template reused across every subject.

## Core principle

Don't generate lessons off a guess. A lesson calibrated to the wrong depth, or built with an exercise shape that doesn't fit the domain (e.g. a prose "practical exercise" for a proof-based math topic, or a toy exercise with no runnable code for a systems topic), is worse than useless — it wastes the user's time and they won't say so, they'll just bounce off it. The interview in Step 1 is not optional filler.

## Step 1: Interview the user

Before writing anything, get clear, concrete answers. Use `ask_user_input_v0` if available (mobile-friendly tappable options) — otherwise ask directly in chat. Don't ask more than what's needed; if the user already answered something in their initial request, don't re-ask it. Batch questions rather than going one at a time.

Required information:
1. **Topic** — specific enough to scope a curriculum. If vague ("machine learning", "get good at programming"), ask them to narrow it or propose a scoped breakdown and confirm before going further (for very broad goals, propose phases first — see "Breaking down a broad mastery goal" below — and build one phase at a time).
2. **Domain shape** — this determines which exercise taxonomy and template features apply (see `references/pedagogy.md`):
   - **CS / algorithms / programming** — code-centric, needs read-before-write sequencing, debugging and tracing exercises, runnable code files
   - **Math** — proof- or computation-centric, needs proper notation rendering, worked-example-before-independent-proof sequencing
   - **Software engineering (practical/industry)** — needs realistic artifacts: code review, design docs, testing, refactoring real-ish code, not toy problems
   - **Mixed** — common for systems topics (e.g. "low-level programming") that are simultaneously CS-conceptual and code-practical; say so and both toolkits apply per-lesson as fits
3. **Current level / background** — not just a self-reported label. Ask what they already know concretely, and for CS/math specifically ask about relevant prerequisites (e.g. "comfortable with recursion and Big-O?", "know basic proof techniques — induction, contradiction?") so lessons aren't built on a false floor.
4. **Depth wanted**:
   - *Survey* — broad map, minimum rigor, "know what it is and how it fits together"
   - *Working proficiency* — enough to actually use it (write real code, apply the math, ship the practice)
   - *Deep mastery* — rigorous, first-principles, edge cases, theory included, "why," not just "how"
   - *Exam/interview prep* — targeted at a specific test or evaluation format
5. **Learning style preference**: explanation-heavy with worked examples / project-code-first / socratic / mixed.
6. **Goal / motivation** — job, project, specific course, curiosity, a deadline. Shapes what to prioritize and cut.
7. **Pace/scope** — roughly how many lessons or how much time. Default 5–10 lessons per phase if unspecified; don't overbuild one phase when the goal is broader (see below).

Also ask what language/tools to assume for programming topics (don't default silently to Python if the user's own stack points elsewhere — check profile/prior context first), and whether this maps to a specific course or exam the user will be assessed on (see Academic integrity, below).

### Breaking down a broad mastery goal

If the user's real goal is broad ("master low-level systems programming", "get real good at math for ML", "become a strong backend engineer") rather than one bounded topic, don't just pick a starting slice and go — first propose a phase breakdown of the whole path (a short ordered list of phases, each a coherent sub-curriculum), confirm the phase order and scope with the user, then build one phase at a time as its own full curriculum (Steps 2–6 below, per phase). This keeps each generated curriculum focused and lets the user redirect between phases instead of committing to a giant plan upfront.

## Step 2: Propose a curriculum outline — confirm before generating

Before writing any HTML, post a plain-text (not file) outline: lesson titles in order, one line each on what each covers, and why that order. Ask the user to confirm or adjust before generating files — cheap to fix here, expensive after 8 HTML files exist.

Sequence by domain:
- **CS**: respect prerequisite dependencies; increase abstraction gradually (concrete mechanism before the general principle it instantiates).
- **Math**: definition → theorem/property → proof → application, with worked examples ahead of anything the learner must prove independently.
- **Software engineering**: increasing scope — a single function, then a module, then a service/system boundary, then cross-cutting concerns (testing, review, design) — rather than covering all scope levels shallowly at once.

Interleave review/application lessons rather than blocking all theory then all practice — interleaving beats blocking for retention regardless of domain.

## Step 3: Research before writing

For "Recommended Reading," use `web_search` to find real, current, reputable sources relevant to that specific lesson — official docs/specs, well-regarded textbooks, canonical papers, primary sources (RFCs, language specs, standards docs) over aggregator blog posts where they exist. Never fabricate a link or title; if nothing solid turns up, omit the section for that lesson. Prefer 2–4 links per lesson, matched to the confirmed depth (a "survey" lesson wants an accessible explainer; a "deep mastery" lesson wants the primary source).

Do not copy substantial text from any source into the lesson — standard copyright limits apply (paraphrase; any direct quote under 15 words; one quote per source max). Lesson content is written by you, in your own words, calibrated to the user's level — not excerpted from search results. For math and technical CS content specifically: verify each claim, formula, or complexity bound before writing it rather than reconstructing it from memory with confidence — a confidently wrong derivation is worse than a flagged uncertainty.

## Step 4: Generate each lesson as linked HTML files (+ real code files where it fits)

Every lesson is a pair: a **content file** and a **practice file** — kept separate so the learner can revisit practice/revision without wading through the full explanation again. Don't merge them, and don't move sections across the split (reading list stays in content; exercise/quiz/revision stay in practice).

Read `assets/lesson_template.html` and `assets/practice_template.html` for structure and CSS — don't design from scratch each time. Read `references/pedagogy.md` for domain-specific guidance on exercises, notation, and what makes each section actually work for CS/math/SWE — read once per session, not per lesson.

**Content file** (`NN-title.html`) must include:
- **Header**: lesson number, title, estimated time, prerequisites
- **Objectives**: 3–5 concrete, testable "by the end of this lesson you can ___" statements
- **Core content**: explanation at the confirmed depth, worked examples before generalization, subheadings, code blocks or math notation as the topic demands (both templates include KaTeX for math — wrap inline math in `\( \)` and display math in `\[ \]`; leave unused if the lesson has no notation)
- **Recommended reading**: real links from Step 3, one-line note on why each is worth reading
- **Nav**: prominent link to the practice file, plus prev/next lesson and index

**Practice file** (`NN-title-practice.html`) must include:
- **Header**: tagged "Practice & Revision," linked back to content
- **Practical exercise**: domain-appropriate exercise type from `references/pedagogy.md` (not one generic shape reused everywhere) — hint (narrows without solving) + collapsible full solution (`<details>`), not shown by default. Skip only if the topic is genuinely non-practical; don't force filler.
- **Self-check quiz**: 3–6 questions tied to the lesson's objectives, collapsible answers with a one-line "why." From lesson 3 onward, include at least one callback question to an earlier lesson (mark it with the `quiz-q callback` class already in the template).
- **Revision notes**: condensed, skimmable, bullets with bolded key terms — the 2-minute re-read, not a repeat of the lesson.
- **Review schedule**: the standard 1/3/7-day spaced-repetition prompt already in the template.
- **Nav**: back to content, prev/next practice, index.

**Runnable code files (CS / SWE / mixed domains, when the exercise involves writing or running real code):** in addition to the HTML pair, generate actual source files the learner compiles/runs locally — `NN-exercise-starter.<ext>` (scaffold with the task set up, TODOs where the learner writes code) and `NN-exercise-solution.<ext>` (full worked solution) — using the language/tools confirmed in Step 1. Link both from the practice file's exercise section. This matters most for anything involving measurement (benchmarks, profiling, assembly inspection) where reading code on a webpage can't substitute for actually running it. Don't generate these for math or purely conceptual lessons.

Save all files to `/mnt/user-data/outputs/`.

## Step 5: Build an index file

One `index.html` listing all lessons with a one-line description and, per lesson, links to content + practice (+ code files if generated). Same visual style as the lessons. Include per-lesson completion checkboxes persisted via `localStorage` — fine here since these are plain files opened directly in the user's browser, not sandboxed chat artifacts.

## Step 6: Present and hand off

`present_files` with the index first, then lessons in order (content, practice, code files) so the index is what's seen first. In chat: briefly confirm the curriculum, explain navigation, and invite depth/pace correction — calibration often needs adjusting after the first real lesson, and that's normal.

## Academic integrity

If Step 1 reveals this maps to a specific course or graded assessment the user will submit, don't write final answers to their actual assignment inside the lesson content — teach the concept and work parallel examples instead, same as any tutoring context. This mode is usually self-directed learning (no professor, no grade), where the only obligation is that the material actually teaches — but check when it isn't.

## Notes on efficient learning (apply throughout, not as an afterthought)

- **Active recall over re-reading**: quizzes and exercises carry more learning value than prose volume. A lesson thin on text but strong on active-recall tasks is usually correct, not a gap to pad.
- **Worked examples before independent problem-solving** (the worked-example effect) — most pronounced for beginners and new concepts; fades for advanced learners on material they already have schemas for, so don't over-scaffold a "mastery" learner revisiting familiar territory.
- **Read before write, for code**: don't ask a learner to produce code cold — show/trace/predict on existing code first (see PRIMM in `references/pedagogy.md`), then modify, then create.
- **Interleaving**: mix review of prior lessons into later ones — a quiz or exercise element calling back to lesson 2 while on lesson 5 — don't silo lessons as islands.
- **Desirable difficulty**: an exercise that's trivial (pure copy-paste) or wildly out of reach (needs untaught material) both fail to teach. Calibrate to the edge of what's been covered so far plus reasonable effort.
- **Concrete before abstract; motivate before explain**: open with why it matters before the dry definition.
- **Don't pad**: a shorter, sharper lesson beats a long one padded to look thorough. If the user asked for "survey" depth, resist writing mastery-depth anyway.
