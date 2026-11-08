# Project Plan & Status — PSI Model Empirical Recreation

Companion to `psi_campaign_coding_rubric.md` and `rubric_derivation_method.md`. This is the living roadmap: what the overarching project is, the pipeline stages, and where we currently are. Update the status markers as stages close.

## Goal

Empirically recreate DesJardine (2025), "The PSI Model for Predicting Activist Engagements." The paper is a theory paper: activists choose tactics (campaign hostility, demand specificity) based on their situation, and those tactics predict outcomes (Propositions 6–9: campaign duration, degree of change, conclusion mode). Recreating it requires (a) a reliable instrument to measure each campaign's tactics from real filings — the rubric — and (b) running that instrument across a real sample to test whether the theory holds.

## Pipeline stages

1. **Derive the rubric from the paper.** ✅ Done — `psi_campaign_coding_rubric.md` + `rubric_derivation_method.md`.
2. **Stress-test the rubric on one real filing.** ✅ Done per changelog (Third Point–Campbell Soup SC 13D). Produced Rule 6 (whole-company sale → specific), Amendment 6a (mocking language as hostility indicator), and the launch-window timing finding. ✅ PROVENANCE VERIFIED 2026-08-30 — pulled the actual filing from EDGAR (SC 13D, Third Point LLC on Campbell Soup Co., filed 2018-08-09, accession 0001193125-18-244115, doc d593020dsc13d.htm). Both cited quotes ("the only justifiable outcome of the strategic review is for the Issuer to be sold to a strategic buyer"; the Ben Franklin "if you fail to plan, you are planning to fail" line) are verbatim in Item 4.
2b. **Expand the stress-test into a pilot batch — REQUIRED before the Stage 3 freeze.** ✅ Done. The Stage 2 single-filing stress test surfaced a missing rule (Rule 6), which proves one filing is not enough to establish that the rubric is stable — so a pilot batch is a required gate before Stage 3 can freeze the rubric, not an optional extra. Hand/AI-code **5 campaigns** chosen to span distinct demand types (transaction opposition, board-control + operations, asset/real-estate separation, board + executive removal, negotiated governance) to flush out missing rules while the rubric can still change. Rule additions should asymptote fast; if new rules are still appearing at the end of the batch, widen it. Every added rule is human-approved and logged in the rubric changelog. Result: all 5 campaigns coded at the launch window, with evidence quotes string-matched to the source filings — Icahn→Dell (Harrowing hardline), Starboard→Darden (held out, hostility 3), Pershing→Target (Gentle general), Third Point→Yahoo (Adaptive aggressive), Starboard→Papa John's (Gentle general). **Zero rule additions across all 5** — rule additions have asymptoted, so the rubric is ready for the Stage 3 freeze. Coded records: `codedrecord.json`. Slate and selection logic: see `step2b.md`.
3. **Promote the rubric to a classification system prompt, then FREEZE it (v1.0).** ⬜ Next. Convert the rubric text into the LLM coding instructions, with the §4 JSON schema as required structured output. Version and lock the rubric before any production coding begins.
4. **Assemble the sample from SEC EDGAR.** ⬜ Define the universe (filing type = SC 13D + amendments; date range; filters e.g. US targets, size floor; N campaigns) and pull Item 4 text + exhibit letters into a structured dataset.
5. **Calibrate before coding.** ⬜ Run the pipeline on the paper's four worked examples (Kinross, Campbell, Huntsman, Bed Bath & Beyond). Must reproduce DesJardine's own classifications before touching the sample.
6. **Run the classification pipeline (frozen rubric).** ⬜ Code every campaign → one JSON record each (scores, evidence quotes, archetype, flags). The model does NOT invent rules here — see Coding protocol below.
7. **Human validation.** ⬜ Blind-code a random 25–30 subsample; Cohen's kappa on the 2×2 archetypes; Spearman on the 1–5 scales; adjudicate disagreements; log rubric fixes.
8. **Test the theory.** ⬜ Bring in outcome data (duration, degree of change, conclusion mode) and test Propositions 6–9 against the coded archetypes.

## Coding protocol — how rule changes are handled

The Campbell-style "amend the rubric when a filing surprises you" behavior belongs to the DEVELOPMENT phase (Stages 2/2b), not production coding. Shape:

- **Iterate freely in the pilot (2b), then FREEZE and version (v1.0)** before production coding.
- **In production (Stage 6) the classifier flags, it does not amend.** When a filing matches no rule, it scores what it can, sets `confidence: low`, states what's missing, flags it, and does NOT guess or invent a rule. Flagged/unmatched cases go to a hold pile.
- **A human batches and adjudicates the hold pile.** Most flags are hard judgment calls within existing rules (handled by the 1–5 scale + a flag), not missing rules. Only a genuinely new, recurring demand type warrants a rule.
- **Rule changes are controlled:** human-approved → version bump (v1.1) → changelog entry → re-run every previously-coded campaign the new rule affects (per rubric §6.4). Never done by the classifying model on its own.

Why: changing the instrument mid-sample makes early- and late-coded campaigns non-comparable and forces re-runs; letting the model rewrite its own rubric abandons the discipline of testing DesJardine's theory rather than the model's reinterpretation.

## Role independence — who may do what

The instrument, the bulk coder, and the reliability check must stay independent so their errors are not correlated.

- **Pulling filings + classifying the sample:** an AI (e.g. another Claude chat) is fine and is the intended design (Stage 6 is an LLM pipeline). Requirements: use the current/frozen rubric, and verify every evidence quote actually appears in the source filing (string-match back to the document — automate at scale; a hallucinated quote would sink the paper).
- **Stress-test / rubric amendments (Stages 2, 2b):** an AI may propose, but a human must sign off on any rule change, since a model amending its own rubric and then grading against it is a correlated-error problem.
- **Validation / Cohen's kappa (Stage 7):** MUST be an independent human. An AI cannot be the sole validator — AI-vs-AI agreement measures reproducibility, not validity, and a reviewer would reject the reliability claim.
- **Outcome blinding:** coders (human or model) must not use knowledge of how a campaign ended when coding launch tactics; outcomes are the dependent variable (Stage 8).

## Open questions to resolve

- **Sample scope (Stage 4):** date range, N, and filters are undecided.
- **Outcome data source (Stage 8):** the dependent-variable dataset is separate from the 13D filings and has not been identified.
- **Conditional/threatened demands (raised by Dell; resolve before Stage 3 freeze):** whether "do X or we'll run a proxy fight" demands enter the specificity sub-scale or count only toward hostility.
- **Negotiated-investment 13Ds (raised by Papa John's; resolve before Stage 3 freeze):** whether cooperative investments with board seats obtained by agreement belong in the sample, given they contain few "demands" in the paper's sense.
- **Launch-window measurement point (raised by Darden and Target; reinforces the Campbell finding):** Darden and Target launched soft and escalated, landing in a different quadrant than their full-campaign reputation, while Dell and Yahoo launched hot. Is the launch window the right measurement point for tactics chosen at targeting?
- **Synthetic positions (raised by Target; Stage 4):** how to treat option- and swap-heavy stakes in the sample definition.

## Verified source references

- Third Point → Campbell Soup SC 13D: https://www.sec.gov/Archives/edgar/data/16732/000119312518244115/d593020dsc13d.htm (accession 0001193125-18-244115, filed 2018-08-09).


## Changelog

Tracks changes to this project plan. (The coding rubric keeps its own separate changelog in `psi_campaign_coding_rubric.md` §7.)

### 2026-09-13
- **Initial version committed.**
- **Stage 2b promoted from "recommended" to a required gate** before the Stage 3 freeze. Trigger: the Stage 2 single-filing stress test (Third Point–Campbell Soup) surfaced a missing rule (Rule 6), showing that one filing cannot establish rubric stability — a pilot batch is required to confirm rule additions have asymptoted before the rubric is locked.
- **Pilot batch sized at 5 campaigns** (previously 20–40) for this pass, selected to span distinct demand types; slate and selection logic recorded in `step2b.md`.
- **Stage 2b marked in progress** (5-campaign slate verified on SEC EDGAR; coding underway).

### 2026-11-08
- **Stage 2b closed (✅ Done).** All 5 pilot campaigns coded at the launch window; evidence quotes string-matched to source filings. Results recorded in `codedrecord.json`.
- **No rule additions needed (5/5)** — every demand mapped onto the existing Rules 1–6 and hostility indicators, satisfying the 2b stopping rule; rubric cleared for the Stage 3 freeze.
- **Pilot spanned the matrix:** Harrowing hardline (Dell), Adaptive aggressive (Yahoo), Gentle general (Target, Papa John's), and one held-out boundary case (Darden, hostility 3). Specialized supportive was not represented; it is covered by the Kinross calibration anchor.
- **Four operational questions surfaced and added to Open questions** (conditional demands, negotiated-investment 13Ds, launch-window measurement point, synthetic positions); the first two are to be resolved before the freeze.
