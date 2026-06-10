---
name: revise-applied-paper
description: Referee and revise an applied economics paper (.qmd, .md, or .tex) against Bellemare (2020), "How to Write Applied Papers in Economics." Three steps — cold review, written referee report, revision with response memo — run once interactively or for several rounds with --loop N. Use when the user asks to revise, review, referee, polish, or check a paper draft, an introduction, abstract, data section, results section, tables, or conclusion against applied-econ writing norms.
---

# Revise an applied economics paper (Bellemare 2020)

Simulate a journal R&R: a **cold referee** reviews the paper against `references/bellemare-checklist.md`, writes a **referee report**, and the paper is then **revised** against that report with a **response memo**. All artifacts persist next to the paper so rounds can build on each other.

Supporting files (relative to this skill's directory):
- `references/bellemare-checklist.md` — the distilled guidance; the referee must apply **every** item.
- `references/bellemare-fulltext.txt` — full text of Bellemare (2020); consult only when the original wording is needed.
- `assets/referee-report-template.md` — the report format; fill it exactly, never invent a different structure.
- `assets/response-memo-template.md` — the response memo format.
- `assets/BellemareHowToPaperSeptember2020.pdf` — source of record; do not read during reviews.

## Arguments

`/revise-applied-paper [file] [--loop N]`

- `file` — the manuscript. If omitted, look in the current directory for `.qmd`, `.md`, or `.tex` files that look like a manuscript (not README, notes, slides). If several candidates exist, ask which.
- `--loop N` — run up to N autonomous rounds (see **Loop mode**). Without it, run **one interactive round**.

## Artifacts

All bookkeeping lives in a `referee/` folder next to the manuscript:

```
referee/
├── referee_report_<YYYY-MM-DD>_r<N>.md   # one per round
├── response_memo_<YYYY-MM-DD>_r<N>.md    # one per round
├── needs_author.md                        # ledger: substantive items only the author can fix
└── declined.md                            # ledger: items the author struck, with reason
```

Get the date with `date +%F`. Determine the round number N by counting existing `referee_report_*` files for this manuscript + 1.

## Step 1 — Cold review

Dispatch the review to a **fresh subagent** (Agent tool, default type) so the referee reads the paper cold, without this conversation's context — real referees haven't watched the author write the draft. Give the subagent ONLY:

1. The manuscript path (instruct it to read the whole file; for `.tex`, also any `\input`/`\include` sub-files; for `.qmd`/`.md`, the YAML front matter).
2. The checklist path: `references/bellemare-checklist.md` — instruct it to walk **every** item, section by section, and record PASS (cite where) / FAIL (cite the offending passage or gap) / N-A (say why) for each.
3. The style sweeps to run across the whole text: past tense for data/results (present tense is the norm outside the conclusion); passive voice; bland enumeration of descriptive means for mere controls; Stata/R code names ("AGE_2", "edu") in tables or prose; too many decimals, scientific notation, unintuitive units; "robust standard errors" without saying robust to what, clustering without level and rationale; causal language anywhere the identification strategy does not support it.
4. **Rounds ≥ 2:** the paths of all prior referee reports, response memos, `needs_author.md`, and `declined.md`, plus these standing rules — *verify whether each prior comment was resolved; do not re-litigate a resolved comment unless the fix is wrong; do not re-raise anything in `declined.md` or `needs_author.md`; rounds ≥ 2 focus on major comments — no new comma-level minor comments after round 1 unless a revision introduced them.*

Have the subagent return findings as a structured list: per finding — checklist item, verdict, severity (major/minor), evidence with location, suggested action, fixable-by-editing (yes/no), and for rounds ≥ 2 a verification verdict per prior comment.

## Step 2 — Referee report

Fill `assets/referee-report-template.md` with the subagent's findings and save it as `referee/referee_report_<date>_r<N>.md`. Severity rules:
- **Major (M1, M2, …):** identification gaps (the three endogeneity sources, SUTVA), missing balance tests / robustness / heterogeneity / mechanisms / limitations, missing data provenance, bait-and-switch or formula-violating introduction, overclaiming, structural problems.
- **Minor (m1, m2, …):** tense, voice, notation, table formatting, decimals, variable names, roadmap, abstract polish — anything fixable by editing alone.

**Interactive mode: STOP here.** Show the user the report (or its summary plus the comment list) and ask which items to strike. Record struck items in `referee/declined.md` with the user's reason. Only surviving items proceed to Step 3.

**Loop mode: do not stop.** The standing rule replaces the checkpoint — see Loop mode below.

## Step 3 — Revise

Work through the surviving report comments one by one, in report order:

- **Apply directly** (Edit) every comment marked *fixable by editing*: tense and voice; section order and names; rewriting the introduction into Head's formula order using only material already in the paper (add a roadmap if missing); tightening the abstract toward hook/question/value-added; table titles, plain-English variable names, significance-symbol notes, decimal consistency (in the source — flag if tables are generated by code elsewhere); equation notation (Greek coefficients, Latin variables, subscripts, no re-used estimand notation). Title changes: propose 2–3 options in the memo; change it only if the user picks one.
- **Park** every comment that needs new analysis, new data, or the author's judgment: append it to `referee/needs_author.md` (comment id, what is needed, why). Never fabricate content to "fix" these — no invented citations, numbers, results, or robustness checks.
- Preserve the source format: `.qmd`/`.md` as Markdown, `.tex` as LaTeX; never break code chunks, cross-references, or citations.

Then fill `assets/response-memo-template.md` mapping **every** comment to applied / parked / declined, and save it as `referee/response_memo_<date>_r<N>.md`.

## Loop mode (`--loop N`)

Run rounds autonomously: Step 1 → Step 2 → Step 3, repeat. Defaults: cap at N rounds (suggest 3 if the user gives no number).

- **Checkpoint replacement:** auto-apply only *fixable-by-editing* comments; everything substantive is parked in `needs_author.md` untouched. Nothing is ever auto-declined — `declined.md` is written only by the user.
- **Stop early** when a round produces **no new major comments** (parked and previously-known items don't count as new).
- **Anti-churn:** every round's referee gets the full prior paper trail and the no-relitigation rules from Step 1.4. If a round proposes reverting a change made in a previous round, do not apply it — flag the conflict in the memo instead.
- **Finish with a consolidated summary** in chat: rounds run and why the loop stopped; everything applied across all rounds (grouped by paper section); the full `needs_author.md` list as the user's to-do; any flagged conflicts.

## Final report (both modes)

End with: where the report, memo, and ledgers live; what changed in the manuscript (by section); what's parked for the author as concrete next actions; and the three acid-test verdicts from the latest report.
