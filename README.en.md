# Extract Skill

Distill books, long videos, and podcasts into callable AI skills.

## Core Capabilities

extract-skill is not a summarization tool — it's an **agent-execution-oriented methodology distillation pipeline**. It turns methodologies in books, long videos, and podcasts into precisely callable skill packs.

| Capability | What it does | Why it matters |
|---|---|---|
| **Triple Verification** | Cross-domain + predictive power + uniqueness checks; pass rate only 25-50% | Filters out quotes and common sense, keeps only reusable methodologies |
| **RIA++ Structuring** | R/I/A1/A2/E/B six-dimension construction; A2 = trigger scenarios, E = executable steps, B = boundaries | Built for agent invocation, not human reading; skills trigger precisely |
| **Pressure Testing** | Independent blind testing with bait prompts and cross-skill confusion tests | Catches trigger-accuracy issues before release, not by luck |
| **Zettelkasten Linking** | depends-on / contrasts-with / composes-with relations + reference graph | Skills compose with each other, not isolated files |
| **darwin Compatible** | Each skill ships with `test-prompts.json` | Output plugs directly into darwin-skill for auto-evolution |

## Why This Exists

You might read many books, save many videos, listen to many podcasts — but struggle to apply any of it. Knowledge stays at the "I've read it" level and never activates in real decisions. Summaries, notes, and transcript cleanup are **compression**, not **reuse**: you still don't know "when to use what." And only a small fraction of high-value content deserves to become a tool — wholesale inclusion dilutes value.

extract-skill has one clear goal: **distill methodologies in long-form content into callable AI skill packs**. It works not only on books but also on videos, podcasts, interviews, lectures, courses, long-form articles, and resource collections with subtitles or transcripts. As long as the content contains extractable, verifiable, transferable methodologies, extract-skill turns it into a set of independently callable, composable, and pressure-testable skills.

To distill video content, pair it with the [video-downloader](https://github.com/kangarooking/kangarooking-skills/tree/main/video-downloader) skill: use it to download videos and extract subtitles/audio transcripts first, then feed the text to extract-skill for methodology extraction, skill construction, and pressure testing.

## What Problems It Solves

| Pain point | How extract-skill solves it |
|---|---|
| Read/watched a lot but can't apply it — knowledge stays at "seen it" | Produces skills with trigger conditions; agent invokes them in real scenarios |
| Summaries/notes are compression, not structured reuse — no "when to use what" | A2 field specifies trigger scenarios + language signals; skills activate on demand |
| Only a small fraction of content deserves to become a tool | Triple verification filters, 25-50% pass rate, removes quotes and common sense |
| Existing methodologies target human readers, not agent executors | RIA++ structure: E = executable steps, B = boundaries; execution-oriented, not consumption-oriented |

## How It Works

extract-skill uses the **RIA-TV++** pipeline to transform a book from raw text into a set of structured skills. The process has seven stages:

1. **Whole-Book Comprehension (Adler Analysis)** — Structural, interpretive, critical, and applicability analysis using Mortimer Adler's method, producing `BOOK_OVERVIEW.md`
2. **Parallel Extraction** — Five specialized extractors (frameworks, principles, cases, counter-examples, glossary) run simultaneously to pull candidate units from the source text
3. **Triple Verification** — Each candidate must pass three checks: at least 2 independent supporting passages (cross-domain), ability to answer a novel question (predictive power), and non-commonsense uniqueness. Pass rate is typically 25-50%
4. **RIA++ Construction** — Verified content is structured into six dimensions: R (original quote) / I (own-words reconstruction) / A1 (book cases) / A2 (future trigger scenarios) / E (executable steps) / B (boundaries & blind spots)
5. **Zettelkasten Linking** — Dependency, contrast, and composition relationships between skills are identified, producing `INDEX.md` with a reference graph
6. **Pressure Testing** — Test prompts including bait questions (and cross-skill confusion tests) are designed for each skill; failures go back for full reconstruction
7. **Delivery** — A reader-facing `DIGEST.md` long-form digest is generated (skip the book, read the essence), and tested skills are installed into the Claude Code / Cursor skills directory so they can actually be invoked

The name RIA-TV++ breaks down as:
- **RIA**: From Zhao Zhou's bookmark method (Reading / Interpretation / Appropriation)
- **TV**: Triple Verification
- **++**: Agent-oriented extensions — E (Execution) + B (Boundary)

## Quick Start

### Prerequisites

- Claude Code, Cursor, or another agent host that supports skill loading
- Source text to distill (PDF / EPUB / TXT / subtitle file / transcript). For videos/podcasts, transcribe first

### Usage

Say any of the following to your agent — extract-skill activates automatically:

```
帮我拆《穷查理宝典》
distill this book into skills: <path>
turn this video/podcast/course into skills
```

The pipeline starts at stage 0, reports progress after each stage, and asks for your confirmation. It supports breakpoint resumption. Full execution spec: [SKILL.md](./SKILL.md). Stage design docs: [methodology/](./methodology/).

## Effect Examples

### Example 1: From a Book to a Skill Pack

**User Need**

"I want to turn a book's core methodologies into reusable AI skills, not just a reading summary."

**How extract-skill reasons**

- Check whether the source material has reusable methodological units
- Distinguish what deserves to be a standalone skill vs. background material
- Output a structured skill repository, not a single summary document

**Example Output**

> The result will not be one summary document. It will be a multi-skill repository with `BOOK_OVERVIEW.md`, `INDEX.md`, a reader-facing `DIGEST.md`, a `GLOSSARY.md`, multiple `*/SKILL.md` files, and `test-prompts.json` for trigger testing.

### Example 2: Structured Reuse, Not Compression

**User Need**

"I don't want a long explanatory article. I want a skill pack my agent can reuse."

**How extract-skill reasons**

- Target is structured reuse, not narrative compression
- Prioritize triggerable, composable, testable skill units
- Reject material that doesn't deserve standalone skill status

**Example Output**

> The system produces multiple skill modules with trigger conditions, boundaries, execution patterns, and related-skill links — rather than flattening the source into one generalized note.

## Generated Skill Packs

| Repository | Source | Skills |
|------------|--------|--------|
| [buffett-letters-skill](https://github.com/kangarooking/buffett-letters-skill) | Buffett's shareholder letters (1957-2023) | 20 |
| [cognitive-dividend-skill](https://github.com/kangarooking/cognitive-dividend-skill) | Cognitive Dividend | 15 |
| [duan-yongping-skill](https://github.com/kangarooking/duan-yongping-skill) | Duan Yongping's Q&A (business + investment logic) | 15 |
| [viral-copywriting-skill](https://github.com/kangarooking/viral-copywriting-skill) | Bao Kuan Wen An | 14 |
| [copywriters-handbook-skill](https://github.com/kangarooking/copywriters-handbook-skill) | The Copywriter's Handbook | 12 |
| [contagious-skill](https://github.com/kangarooking/contagious-skill) | Contagious | 15 |
| [influence-skill](https://github.com/kangarooking/influence-skill) | Influence | 12 |
| [1000-true-fans-skill](https://github.com/kangarooking/1000-true-fans-skill) | 1000 True Fans | 13 |
| [system-prompt-skills](https://github.com/kangarooking/system-prompt-skills) | 165 AI product system prompts | 15 |
| [X-growth-skills](https://github.com/kangarooking/X-growth-skills) | Practical X (Twitter) account launch, content growth, algorithm, engagement, and monetization resources | 15 |
| [poor-charlies-almanack-skill](https://github.com/kangarooking/poor-charlies-almanack-skill) | Poor Charlie's Almanack | 12 |
| [no-rules-rules-skill](https://github.com/kangarooking/no-rules-rules-skill) | No Rules Rules | 10 |
| Huangdi Neijing Suwen (in this project) | Huangdi Neijing: Suwen | 10 |
| Huangdi Neijing Lingshu (in this project) | Huangdi Neijing: Lingshu | 8 |
| [first-principles-skill](https://github.com/kangarooking/first-principles-skill) | First Principles | 10 |
| [mao-selected-works-skill](https://github.com/kangarooking/mao-selected-works-skill) | Selected Works of Mao Zedong, Vol. 1-5 | 25 |
| [qbdx-hub/buffett-letters-skill](https://github.com/qbdx-hub/buffett-letters-skill) | Buffett Shareholder Letters (1957-2023) | 20 |
| [qbdx-hub/wo-yu-di-tan-skill](https://github.com/qbdx-hub/wo-yu-di-tan-skill) | Wo Yu Di Tan | 6 |
| [qbdx-hub/mingchao-those-things-skill](https://github.com/qbdx-hub/mingchao-those-things-skill) | Mingchao Those Things | 7 |
| [qbdx-hub/sunzi-bingfa-skill](https://github.com/qbdx-hub/sunzi-bingfa-skill) | Sunzi Bingfa | 8 |
| [qbdx-hub/zhouyi-skill](https://github.com/qbdx-hub/zhouyi-skill) | Zhouyi | 8 |
| [qbdx-hub/high-math-vol1-ch1-skill](https://github.com/qbdx-hub/high-math-vol1-ch1-skill) | High Math Vol. 1 Chapter 1 | 8 |

More high-value books are planned for distillation.

Additional external source (included with the author's permission):

- Source repository: [ace3000chao/book2startup](https://github.com/ace3000chao/book2startup)
- Included books: *The Lean Startup*, *The Art of War*, *Zhuangzi*, and *I Ching*
- Source repository: [shenqistart/book2skill](https://github.com/shenqistart/book2skill)
- Included books: *Chanlun* and *The Classic of Tea*

## Repository Structure

```text
extract-skill/
├── README.md              ← You are here
├── README.en.md           ← English version
├── README.ja.md           ← Japanese version
├── LICENSE                ← MIT
├── SKILL.md               ← Meta-skill definition (full execution spec for extract-skill)
├── methodology/           ← RIA-TV++ stage-by-stage methodology docs
├── extractors/            ← Prompt definitions for the 5 parallel extractors
└── templates/             ← SKILL / INDEX / BOOK_OVERVIEW / DIGEST / GLOSSARY / verified / PIPELINE_STATE / test-prompts templates
```

## Ecosystem

extract-skill is part of a larger skill ecosystem:

- [nuwa-skill](https://github.com/alchaincyf/nuwa-skill) — Distills people (thinking styles, expression DNA)
- **extract-skill** (this repo) — Distills books (methodologies, frameworks, principles)
- [darwin-skill](https://github.com/alchaincyf/darwin-skill) — Evolves any skill

They interlock: nuwa distills people, extract distills books, darwin keeps them evolving.

## More Skills

- [Buffett Letters Skill](https://github.com/kangarooking/buffett-letters-skill) — 20 investment reasoning skills from Buffett's 60+ years of shareholder letters
- [Poor Charlie's Almanack Skill](https://github.com/kangarooking/poor-charlies-almanack-skill) — 12 decision-making and judgment skills from Charlie Munger's core thinking methods
- [No Rules Rules Skill](https://github.com/kangarooking/no-rules-rules-skill) — 10 organizational design skills from Netflix's culture of freedom and responsibility
- [Cognitive Dividend Skill](https://github.com/kangarooking/cognitive-dividend-skill) — 15 cognitive tool skills for thinking upgrades from Cognitive Dividend
- [Duan Yongping Skill](https://github.com/kangarooking/duan-yongping-skill) — 15 business and investment skills from Duan Yongping's Q&A collection
- [Viral Copywriting Skill](https://github.com/kangarooking/viral-copywriting-skill) — 14 sales copywriting and diagnosis skills from *Bao Kuan Wen An*
- [Copywriters Handbook Skill](https://github.com/kangarooking/copywriters-handbook-skill) — 12 sales copywriting, headline, and benefit translation skills from *The Copywriter's Handbook*
- [Contagious Skill](https://github.com/kangarooking/contagious-skill) — 15 STEPPS propagation strategy and word-of-mouth diagnosis skills from *Contagious*
- [Influence Skill](https://github.com/kangarooking/influence-skill) — 12 persuasion psychology, compliance mechanism, and defensive judgment skills from *Influence*
- [1000 True Fans Skill](https://github.com/kangarooking/1000-true-fans-skill) — 13 personal branding, true fan development, and trust-based monetization skills from *1000 True Fans*
- [System Prompt Skills](https://github.com/kangarooking/system-prompt-skills) — 15 system prompt design skills distilled from 165 AI product system prompts
- [X Growth Skills](https://github.com/kangarooking/X-growth-skills) — 15 skills for X account launch, content, algorithms, engagement, review, and monetization
- Huangdi Neijing Suwen Skill (in this project) — 10 traditional Chinese medicine observation and regulation skills from *Huangdi Neijing: Suwen*
- Huangdi Neijing Lingshu Skill (in this project) — 8 body-mind regulation and syndrome differentiation skills from *Huangdi Neijing: Lingshu*
- [First Principles Skill](https://github.com/kangarooking/first-principles-skill) — 10 skills on axiomatic reasoning, boundary-breaking innovation, and organizational refresh from *First Principles*
- [Mao Selected Works Skill](https://github.com/kangarooking/mao-selected-works-skill) — 25 cognition, strategy, organization, and execution skills from *Selected Works of Mao Zedong*
- [qbdx-hub Buffett Letters Skill](https://github.com/qbdx-hub/buffett-letters-skill) — 20 investment and capital allocation skills from Buffett shareholder letters
- [qbdx-hub Wo Yu Di Tan Skill](https://github.com/qbdx-hub/wo-yu-di-tan-skill) — 6 skills on limits, suffering, writing, and self-anchoring from *Wo Yu Di Tan*
- [qbdx-hub Mingchao Those Things Skill](https://github.com/qbdx-hub/mingchao-those-things-skill) — 7 skills on power structure, institutional failure, and historical explanation from *Mingchao Those Things*
- [qbdx-hub Sunzi Bingfa Skill](https://github.com/qbdx-hub/sunzi-bingfa-skill) — 8 skills on strategic judgment, resource control, and action selection from *Sunzi Bingfa*
- [qbdx-hub Zhouyi Skill](https://github.com/qbdx-hub/zhouyi-skill) — 8 skills on situational diagnosis, timing, and advance-retreat boundaries from *Zhouyi*
- [qbdx-hub High Math Vol. 1 Chapter 1 Skill](https://github.com/qbdx-hub/high-math-vol1-ch1-skill) — 8 learning skills on limits, infinitesimals, and continuity from High Math Vol. 1 Chapter 1

External Source (included with the author's permission):

- [book2startup](https://github.com/ace3000chao/book2startup) — includes skills distilled from *The Lean Startup*, *The Art of War*, *Zhuangzi*, and *I Ching*
- [book2skill](https://github.com/shenqistart/book2skill) — includes AI-Agent skills distilled from *Chanlun* and *The Classic of Tea*

## License

MIT. See [LICENSE](./LICENSE).
