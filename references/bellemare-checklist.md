# Bellemare (2020) — "How to Write Applied Papers in Economics" — Full Guidance Checklist

Source: Marc F. Bellemare, *How to Write Applied Papers in Economics*, September 7, 2020 (chapter from forthcoming MIT Press book, *Doing Economics*).

Every item below is a check to apply against the paper under review. Each is phrased so it can be answered PASS / FAIL / N-A, with evidence from the manuscript.

---

## 1. Overall structure (§2)

- [ ] The paper follows (or consciously departs from) the canonical applied-econ structure:
  1. Title, 2. Abstract, 3. Introduction, 4. Theoretical Framework, 5. Data and Descriptive Statistics, 6. Empirical Framework, 7. Results and Discussion, 8. Summary and Concluding Remarks, 9. References, 10. Appendix.
- [ ] Acceptable variations: Empirical Framework before Data; no Theoretical Framework when the theory is textbook-standard; a Background section after the Introduction when contextual detail fits nowhere else.
- [ ] If structure departs from the norm, the departure is deliberate and justified — "before you break the rules, you have to learn them."

## 2. Single research question and focus (§3)

- [ ] The paper focuses on a single, narrow research question (or the mechanisms behind one), not a scattershot of outcomes. Material not serving that question belongs on the cutting-room floor.

## 3. Theoretical framework (§3)

- [ ] If the theory of change is already in the literature: the paper either adopts or adapts an existing framework, **clearly stating** that it does so. Novelty is required in the question or empirical strategy, not on every front.
- [ ] If the theory of change is new: it is clearly stated — formal model or verbal conceptual framework — starting from primitives and making just the assumptions needed to generate "x causes y through mechanism m," **no more and no less**.
- [ ] Better an informal, chatty conceptual framework than a bad formal model. A verbal argument with math relegated to an appendix is acceptable.

## 4. Data and Descriptive Statistics (§4)

The section must answer **all** of the reader's questions about the data:

- [ ] Where the data come from, when collected, and by whom.
- [ ] How observations were chosen (survey methodology; how regions/communities/households/individuals were selected).
- [ ] What population the sample is representative of (external validity).
- [ ] Target sample size and how it was determined (e.g., power calculations); actual sample size.
- [ ] Nonresponse rate; attrition rate if longitudinal.
- [ ] How missing data were handled (dropped vs. imputed, and how imputed).
- [ ] Every variable used in the paper — and **no variable not used** — is introduced, with precise and concise explanation of what it measures and how (e.g., what the components of "income" are; this can hide reverse-causality problems).
- [ ] Ideally a table of variable descriptions: one row per variable; column 1 = name (unit in parentheses), column 2 = precise definition.
- [ ] Table of means and standard deviations is present.
- [ ] **Balance tests** when the treatment has few categories: means and SEs by treatment status with p-values of pairwise difference-in-means tests, for **every pairwise comparison** of treatment arms. Experimental data: shows randomization worked (expect <1/10 of comparisons significant at 10%, <1/20 at 5%, <1/100 at 1%). Observational data: assesses how unbalanced the data are; if badly unbalanced, the relevant covariates should be controlled for downstream.
- [ ] Where useful, nonparametric exploration: kernel densities for continuous outcome/treatment variables, histograms for categorical ones, cross-tabs when both treatment and outcome are binary.

Mistakes to flag:

- [ ] No bland enumeration of means ("37.4% of respondents are female") for mere controls — discuss only the outcome, the treatment, identification variables (instruments, forcing variables), or anything that genuinely stands out. Keep the descriptive discussion to a few sentences.
- [ ] **Present tense**, not past tense, for data and results ("37.4 percent of respondents *are* female"). Past tense only when summarizing and concluding. Avoid the passive voice throughout.
- [ ] Numbers readable: at most ~3 decimal places; rescale variables rather than print tiny magnitudes or scientific notation (no `1.37e+8`); use units readers know (dollars in thousands, etc.).
- [ ] Descriptive statistics report the **level** of a variable (e.g., mean income), even if the regression uses its log.
- [ ] The section lets the reader form reasonable expectations about the sign and magnitude of the causal effect of interest.

## 5. Empirical framework (§5)

### 5.1 Estimation strategy

- [ ] The estimated equations are written out explicitly — the reader should never have to reverse-engineer them from tables.
- [ ] Equations are parsimonious: dependent variable `y`, treatment `D` (or `T`), vector of controls `x`, intercept `α`, error `e`; the 10–15 individual controls go into the vector, not the equation.
- [ ] All variables carry proper subscripts (i, j, k, … from smallest to largest unit).
- [ ] Latin letters denote variables; Greek letters denote coefficients.
- [ ] No re-used estimand notation across specifications: different specifications ⇒ differently subscripted coefficients (β₀, β₁ or βᵣ, βₛ).
- [ ] The estimation method for each equation is stated (LPM vs. probit vs. logit for binary outcomes; OLS/ML/GMM where ambiguous).
- [ ] Relevant hypothesis tests are stated (e.g., H₀: γ = 0 vs. Hₐ: γ ≠ 0) — remembering a statistical test always tests an equality.
- [ ] Inference is fully described: robust SEs robust **to what** (say "robust to heteroskedasticity," not just "robust"); clustered at what level **and why** (Abadie et al. 2017); sampling weights used or not, and how constructed (Solon et al. 2015).

### 5.2 Identification strategy

- [ ] There is an explicit discussion of how the coefficient of interest is causally identified.
- [ ] Experimental variation + balanced ⇒ short section is fine. Experimental but unbalanced ⇒ explain controls added and acknowledge unobservables likely also unbalanced.
- [ ] Observational data ⇒ the paper explains intuitively why the results have a shot at causal identification — why these results get closer than the existing literature.
- [ ] The **three sources of statistical endogeneity** are each discussed in turn: (i) reverse causality, (ii) unobserved heterogeneity, (iii) measurement error — whether each is a concern here, how it is dealt with, and how it might bias the coefficient.
- [ ] **SUTVA** violations are considered (spillovers across units, across time, or both), and tested for where possible.
- [ ] No overclaiming: if results are not causally identified, the paper does not pretend they are. Theoretical exogeneity is not statistical exogeneity. Candid admission of limitations beats attempted deception — editors and reviewers reward candor.

## 6. Results and Discussion (§6)

### 6.1 Order of results
- [ ] Results progress from most parsimonious (bivariate y on D) to least parsimonious (full controls). With observational data, coefficient stability as controls are added is itself a first robustness check (Altonji et al. 2005; Oster 2019).

### 6.2 Robustness checks
- [ ] Alternative measurements of the outcome are exploited where available (e.g., the many measures of "welfare").
- [ ] Alternative measures of the treatment where available (extensive vs. intensive margin).
- [ ] Placebo tests (fake treatment, correlated with the treatment but not causing the outcome) and/or falsification tests (fake outcome) where feasible — robustness shown by the **absence** of significance.
- [ ] Alternative estimators / flexible functional forms (e.g., restricted cubic splines for a continuous treatment) where useful.

### 6.3 Treatment heterogeneity
- [ ] Heterogeneity across relevant sub-groups (gender, rural/urban, income quintiles, …) is explored — average effects can mask it, and heterogeneity can salvage a null finding.

### 6.4 Mechanisms
- [ ] The paper does its best to investigate mechanisms ("How does D cause y?"): proper mediation analysis in the best case (Acharya et al. 2016); descriptive regressions or correlations clearly labeled as not causally identified otherwise; and where mechanisms cannot be tested even by proxy, the paper says so explicitly and explains why.

### 6.5 Limitations
- [ ] A dedicated limitations discussion (ideally its own sub-section of the results section) covering, as relevant: (i) internal validity limits (e.g., only plausibly exogenous IV — with sensitivity analysis à la Conley et al. 2012), (ii) external validity limits (lab / lab-in-the-field / RCT context; nebulous complier populations under LATE), (iii) proxy variables standing in for the construct of true interest.

### 6.6 Tables
- [ ] Table titles are self-explanatory: estimator + causal relationship + sub-sample (e.g., "OLS Results for the Effect of Participation in Contract Farming on Household Income, by Gender").
- [ ] Coefficients and SEs have the **same number of decimals** throughout (usually 2–3).
- [ ] Working papers show all controls; if a "Controls? Yes" line is used, the table notes list exactly which controls (readers will check for colliders and bad controls on the causal path).
- [ ] Bottom lines report N, R² (plain, not adjusted), joint-significance tests where relevant, and indicator lines for fixed effects / trends.
- [ ] Notes define **all** significance symbols (*, **, *** at 10/5/1%), plus extra symbols (†, ††, †††) if p-values are adjusted, SEs bootstrapped, or randomization inference used.
- [ ] **Same estimation sample across specifications**: use the smallest sample (dictated by missingness) for all columns, never a shrinking N as controls are added.
- [ ] Variable names in plain English ("Years of education," "Female"), never code names ("Edu," "AGE_2," "SEX").
- [ ] The acid test: given only the tables, could a reader write down the exact regression estimated?

## 7. Summary and Concluding Remarks (§7)

The conclusion should contain, in order:

- [ ] **Summary** — different enough from the abstract and introduction; tell a story if possible.
- [ ] **Limitations** — re-emphasized even if a limitations sub-section exists in the results.
- [ ] **Implications for policy** — supported by the results only; attempt a back-of-the-envelope cost–benefit comparison; identify winners and losers in two or three sentences; briefly assess political feasibility.
- [ ] **Implications for future research** — how internal/external validity could improve; stage-setting for follow-up work if any.

## 8. Title, Abstract, Introduction (§8)

### 8.1 Title
- [ ] Not technique-focused ("A Semiparametric Investigation…" is a repellent unless the paper develops the technique).
- [ ] Not overlong (short titles get cited more — Letchford et al. 2015).
- [ ] Safe norms: "The Impact of D on y: Evidence from [Context]" or "D and y".
- [ ] If clever/cute: it must appeal broadly, actually make sense (use the familiar form of sayings), and fit the paper like a glove.
- [ ] Starting with "On…" is fine, contrary to old folk advice.

### 8.2 Introduction — Keith Head's formula, in this exact order
- [ ] **Hook** (1–2 paragraphs): grabs attention, relates to the real world; never "A long literature in economics has looked at…".
- [ ] **Research question** (1 paragraph): the actual question stated as clearly as possible, ideally as the paragraph's first sentence.
- [ ] **Antecedents**: relate the paper to the 5–10 closest studies (closer to 5 the better), told as a story — not a bland "X (2011) found this. Y (2012) found that."
- [ ] **Value added**: the contribution(s), ideally three (e.g., internal validity, external validity, a modest methodological improvement); how the paper changes priors.
- [ ] **Roadmap**: "The remainder of this paper is organized as follows…" — include it; delete only if a reviewer asks.
- [ ] No bait-and-switch: the introduction must not overpromise relative to what the paper delivers.
- [ ] Important facts from any skippable section (e.g., Background) are also stated in the introduction, since busy readers read title → abstract → introduction → tables → conclusion. The greatest sin is omission; the second greatest is making the reader hunt for information.

### 8.3 Abstract
- [ ] Drafted by keeping roughly the first sentences of the hook, research question, and value-added parts of the introduction, polished into one paragraph.
- [ ] Intelligible to any smart, college-educated non-economist, except requisite terminology (RCT, diff-in-diff, RD). Do not confuse unintelligibility with rigor.

## 9. Literature review and background sections (§9)

- [ ] **No standalone literature-review section** in a journal article (theses/dissertations excepted) — the mini-review in the introduction's antecedents does that job.
- [ ] A **Background section** only when genuine contextual knowledge is required (legislation details, industry description) — telling the reader what she needs to know, no more and no less.

## 10. Writing style — cross-cutting

- [ ] Present tense for data and results; past tense only in summary/conclusion.
- [ ] Active voice; avoid the passive.
- [ ] Write for a PhD economist *outside* the field (the first-year-core classmate) — general-audience clarity helps even at field journals.
- [ ] Minimize the work the reader must do at every turn; the opportunity cost of a reader's time is high.
- [ ] Honesty throughout: never make claims the research design and results cannot support.

## 11. Where to submit (§10) — apply only if the user asks about journal targeting

- [ ] If targeting a field journal, cite a good number of recent (last ~5 years) articles from that journal or close substitutes — this guides desk-reject decisions and reviewer selection.
- [ ] Journals cited 3+ times in the reference list are natural candidate outlets.
- [ ] Improvements on internal **and** external validity justify trying a general journal first.
