# revise-applied-paper

A [Claude Code](https://claude.com/claude-code) skill that referees and revises an applied
economics paper (`.qmd`, `.md`, or `.tex`) against the writing standards in Marc F. Bellemare's
*[Doing Economics](https://mitpress.mit.edu/9780262543552/doing-economics/)* (MIT Press, 2022).
Think of it as an **AI paper referee**: it acts like an automated journal reviewer, producing a
referee report and then revising the manuscript against that feedback.

**What sets it apart from other AI writing tools:** its standard of reference. Instead of
improvising what "good academic writing" means, every comment is anchored to a published, widely
cited authority in applied economics — Bellemare's book — not to a model's private taste.

> 📄 For a longer write-up — the motivation, the workflow stage by stage, and notes on cost —
> see the [project description page](https://noejn2.github.io/project-descriptions/revise-applied-paper).

> [!IMPORTANT]
> **This is a writing-craft tool, not scientific peer review.** It helps the *practitioner*
> write a better paper by checking the manuscript against Bellemare's recommendations on
> structure, exposition, and presentation — does the introduction follow Head's formula, is the
> data section complete, are the tables self-explanatory, does the abstract read for a general
> audience. The "referee report" it produces is about **how the paper is written**, not about
> whether the economics is correct. It does **not** judge the validity of your identification,
> the soundness of your model, the correctness of your results, or the merit of your
> contribution. Treat its output as an editor's read for clarity and convention — never as a
> verdict on the scientific or academic substance of the work. That judgment remains yours and
> your actual referees'.

## What it does

Applied papers get rejected for avoidable writing reasons — a buried research question, a thin
data section, an introduction that overpromises, tables a reader can't reconstruct. Bellemare
wrote down the unspoken norms that prevent this in *Doing Economics*. This skill turns those norms
into a repeatable check on your own draft, structured as a journal R&R.

## How it compares

Plenty of AI tools will critique your writing. The difference here is the **standard of reference**
above: this skill audits your draft against a published, widely cited book on how applied
economists actually write, so its comments trace back to an authority you can cite — not to a
model's improvised opinion. The rest of the trade-off against hosted, paid reviewers looks like
this:

![How revise-applied-paper compares to hosted AI reviewers](assets/venn.png)

Both approaches overlap on the basics — AI-powered review, writing and clarity checks, citation
gaps, pre-submission feedback. They diverge on everything else:

- **revise-applied-paper** (*open · free · yours*) is anchored to that citable authority, runs
  locally so your unpublished draft never leaves your machine, costs $0 on top of Claude, is open
  to fork and adapt, teaches as it reviews, and stays interactive — you push back on any comment
  and iterate live, on a whole draft or section by section, at your own pace.
- **Hosted alternatives** (*paid · hosted · deep compute*) trade those for raw horsepower: hours
  of compute, whole-argument stress tests, deep proof-chasing, and a polished, shareable report
  with no setup — at the cost of uploading your unpublished work to someone else's servers.

In short: reach for a hosted reviewer when you want a heavyweight, one-shot stress test; reach for
this when you want an everyday, private, authority-grounded craft check you control.

## How it works

Three steps, simulating a round of review:

1. **Cold review** — a fresh subagent reads your manuscript *without* your conversation context
   (like a real referee who didn't watch you write it) and audits it against an ~80-item
   checklist distilled from Bellemare's *Doing Economics*.
2. **Referee report** — the findings are written up as a referee report — a summary of the paper,
   numbered **major** comments (structure, data provenance, bait-and-switch, overclaiming) and
   **minor** comments (tense, notation, tables, abstract polish) — and saved next to your paper.
3. **Revise** — editing-level comments are applied to the manuscript; substantive ones that need
   new analysis or your judgment are *parked* in a to-do ledger (never fabricated); every comment
   is answered in a response memo.

Run it once **interactively** (it shows you the report and lets you strike items before any edit),
or in **loop mode** (`--loop N`) where it runs several autonomous rounds — each new round verifies
the previous round's fixes and stops when a round raises no new major comments.

Everything persists in a `referee/` folder beside your paper, so the reports are diffable across
drafts and double as a rehearsal for your real response-to-reviewers later:

```
referee/
├── referee_report_<date>_r1.md   # one per round
├── response_memo_<date>_r1.md    # every comment → applied / parked / declined
├── needs_author.md               # substantive items only you can address
└── declined.md                   # items you struck, with your reason
```

## Usage

```bash
/revise-applied-paper article.qmd            # one interactive round
/revise-applied-paper article.qmd --loop 3   # up to 3 autonomous rounds
```

It also triggers automatically when you ask things like *"review my introduction"* or *"check
this draft against applied-econ norms."*

## Performance & cost

The skill is **token-intensive**: each round dispatches a fresh referee that re-reads the
manuscript, the checklist, and the style sweeps, and an autonomous loop repeats that across
agentic turns. A few rules of thumb to keep runs cheap without losing the value:

- **`--loop 1` or `--loop 2` captures most of the benefit.** The first round surfaces the
  structural comments that matter; later rounds mostly verify fixes and yield diminishing returns.
- **Shorter manuscripts cost less per round** — run it on a section (an introduction, a data
  section) when you don't need a full-paper pass.
- **Autonomous runs are cheaper than mid-session interruptions** — kicking off `--loop N` on a
  clean manuscript avoids re-reading a long conversation transcript on every turn.

## Example — a real run

Run on an actual working paper (*"The cost of common simplifications in censored demand-system
estimation"*), in loop mode with a cap of three rounds.

### 1. One command, loop engaged

The skill dispatches a fresh cold referee for Round 1 — it reads only the manuscript, the
checklist, and the style sweeps, with none of the author's chat context.

![Invocation and cold-review dispatch](assets/demo-screenshots/demo-1.png)

### 2. The loop converges and reports back

The loop ran **2 of up to 3 rounds and stopped early** — Round 2's referee verified every Round-1
edit and found no new major comments. The summary lists what was applied to the manuscript by
section, and what was parked for the author. Real comments from the report behind this run:

> **M1. Data provenance is incomplete for a real-survey application.** §sec-data names "ENIGH 2018"
> and "roughly 61,600 households" but gives no fielding agency, collection dates, sampling design,
> or the rules that produced n=61,599. *Fixable by editing: no — needs the author's knowledge.*

> **M5. Raw-vs-corrected price framing risks a bait-and-switch.** The abstract promises "the real
> ENIGH 2018 data behind Nava & Dong (2022)," but the body presents only raw unit-value results and
> a table caption points to a corrected-price comparison the body never shows.

> **m3.** [abstract] Dense with unglossed jargon for a non-economist — "Tobit," "Stone price index,"
> "translog," "QUAIDS," "bias floor" → lightly gloss the most technical terms.

![Consolidated loop summary](assets/demo-screenshots/demo-2.png)

### 3. You stay in control

The report is a checkpoint, not an autopilot. Here the author declines one comment —
*"Drop M3, ENIGH is very well known in the censored demand literature"* — and it's recorded in
`declined.md` with that reason while the rest are applied. Note the skill **read the actual repo
rather than fabricating**, which surfaced a real finding, and it changed only prose — *"no estimate,
number, or claim was changed anywhere."*

![Declining an item and final wrap-up](assets/demo-screenshots/demo-3.png)

What landed in the manuscript were writing fixes (provenance paragraph, a variable-definition table,
plain-English variable names, glossed abstract, a consistent decimal format). What it would *not*
touch — and parked for the author instead — were things requiring new analysis or judgment
(regenerating tables, a cold-start Monte-Carlo, the weighting decision). That line is exactly the
point of the disclaimer above.

## Installation

The skill must live in `~/.claude/skills/` to be active. Clone it there, or clone anywhere and
symlink:

```bash
git clone git@github.com:noejn2/revise-applied-paper.git
ln -s "$(pwd)/revise-applied-paper" ~/.claude/skills/revise-applied-paper
```

Then `/revise-applied-paper <your-draft>` in any project.

## Repo layout

```
revise-applied-paper/
├── SKILL.md                       # the skill: workflow + interactive/loop modes
├── references/
│   ├── bellemare-checklist.md     # the ~80-item audit, distilled from the paper
│   ├── bellemare-fulltext.txt     # full text, for when original wording is needed
│   └── uw-abstract-guide.md       # abstract criteria from the UW–Madison Writing Center
└── assets/
    ├── referee-report-template.md # fixed report format
    ├── response-memo-template.md  # fixed memo format
    └── BellemareHowToPaperSeptember2020.pdf
```

## Credit

All of the substance here is Marc F. Bellemare's. The checklist distills the chapter on writing
applied papers from his book
*[Doing Economics](https://mitpress.mit.edu/9780262543552/doing-economics/)* (MIT Press, 2022).
For the real thing, read the book and his
[blog](http://marcfbellemare.com/wordpress/) on academic writing — and Keith Head's
[Introduction Formula](https://blogs.ubc.ca/khead/research/research-advice/formula), which it
relies on. The abstract criteria in `references/uw-abstract-guide.md` are distilled from the
University of Wisconsin–Madison Writing Center's
[Writing an Abstract for Your Research Paper](https://writing.wisc.edu/handbook/assignments/writing-an-abstract-for-your-research-paper/).
