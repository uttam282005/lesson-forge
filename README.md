# lesson-forge

A Claude skill that turns "teach me X" into an actual curriculum — not a wall of chat text, but a set of linked, self-contained HTML lessons you open, work through, and revisit.

## Why

Most "teach me X" requests from an AI produce one long explanation you skim once and never look at again. lesson-forge instead:

- **Interviews you first** — depth, learning style, background, goal — before writing anything, because a lesson calibrated to the wrong level is worse than useless.
- **Confirms the curriculum outline** before generating files, so structure gets fixed while it's cheap to fix.
- **Sources real reading links** via web search instead of inventing them.
- **Separates explanation from practice** — each lesson is two linked files, so you can drill the exercise or skim revision notes later without re-reading the whole lesson.
- **Bakes in learning-science basics** by default: active recall (quizzes before answers), worked examples before abstraction, interleaved review of earlier lessons, and a spaced-repetition (1/3/7-day) prompt on every lesson — instead of leaving them as an afterthought.

## What it produces

For a curriculum of *N* lessons, you get `2N + 1` files:

```
index.html                          ← curriculum home, links + progress checkboxes
01-lesson-title.html                ← objectives, core content, recommended reading
01-lesson-title-practice.html       ← exercise, self-check quiz, revision notes, review schedule
02-lesson-title.html
02-lesson-title-practice.html
...
```

All files share consistent CSS, are fully self-contained (no build step, no server), and use `localStorage` for per-lesson completion tracking — just open `index.html` in a browser.

## How it works

1. **Interview** — topic, current level, depth (survey / working proficiency / deep mastery / exam prep), learning style (explanation-heavy / project-first / socratic / mixed), goal, and rough pace.
2. **Outline proposal** — a plain-text lesson-by-lesson outline, posted for confirmation before any file is generated.
3. **Research** — real, current sources looked up per lesson for the reading list; nothing fabricated.
4. **Generation** — each lesson written as a content/practice file pair from the shared templates, calibrated to the confirmed depth and style.
5. **Index + handoff** — a linking index file, presented alongside the lessons.

See [`SKILL.md`](./SKILL.md) for the full instructions the agent follows, and [`references/pedagogy.md`](./references/pedagogy.md) for the underlying pedagogy notes.

## Installation

Drop the `lesson-forge/` directory (or the packaged `.skill` file) into your Claude skills directory, or upload it wherever your Claude client supports custom skills.

```
skills/
└── lesson-forge/
    ├── SKILL.md
    ├── assets/
    │   ├── lesson_template.html
    │   └── practice_template.html
    └── references/
        └── pedagogy.md
```

## Usage

Just ask, in chat:

> Teach me [topic]

or

> Help me learn [topic] / make me a course on [topic] / I want to get good at [topic]

The skill takes it from there — expect a few clarifying questions, then an outline to confirm, then the files.

**Not** for a single quick question ("what is a hash map?") — it's built for multi-lesson curricula. One-off explanations should just be answered directly.

## Customization

- Edit `assets/lesson_template.html` / `assets/practice_template.html` to change the visual style — both files share one CSS approach, so edit them together.
- Edit `references/pedagogy.md` to change how sections get filled in (exercise difficulty, quiz style, revision-note density) without touching the core workflow in `SKILL.md`.
- The interview questions and required fields live in `SKILL.md` Step 1 — adjust if you want different defaults (e.g. a fixed depth level, a fixed lesson count).

## License

MIT (or match whatever license the rest of your skills repo uses).
