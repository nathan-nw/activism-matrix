# How the Coding Rubric Was Derived from DesJardine (2025)

Companion document to `psi_campaign_coding_rubric.md`. This records the derivation method so the operationalization can be defended and reproduced — the answer to "how did you turn my constructs into codes?"

## Step 1 — Locate the defining passages

For each construct, isolate the sentences where the author *defines* rather than *discusses* it.

**Demand specificity** is defined as "the degree of detail and explicitness with which a shareholder outlines *the entirety of* operational or strategic actions that they want to see executed." The phrase "the entirety of" carries the analytical weight, and it becomes the rubric's core test: does the demand bound the total set of actions the company must take, or does it leave the eventual changes to management's discretion post-engagement?

**Campaign hostility** is defined by "the nature in which activists make their demands" — the *manner* of engagement, not the *content* of the ask. This single distinction generates the rubric's most important rule: code the manner, not the ask. A demand that is aggressive in substance (remove the CEO) can be delivered relationally (privately, with praise for prior service), and the rubric must keep those separable.

## Step 2 — Extract the author's own decision rules

Theory papers often state operational rules in passing. Three appear explicitly in the paper and were transcribed into the rubric as non-overridable rules:

1. Board composition demands are always **broad**, even when the activist names candidates, because the directors' eventual strategic and operational impact is undefined at demand time.
2. Executive/director removal demands are **broad and hostile simultaneously** — broad because the resulting strategic change is unspecified, hostile because they name specific people.
3. Even removal demands can be delivered in a **relational manner** (private engagement, praise for the targeted director's service record), so delivery is coded independently of the demand's content.

Using the author's rules rather than the coder's intuitions ensures the empirical exercise tests *his* theory, not a reinterpretation of it.

## Step 3 — Convert the paper's worked examples into scale anchors

The paper supplies one worked case per quadrant of the activism matrix, plus two style-switch anecdotes:

| Case | Role in rubric |
|---|---|
| Elliott Management → Kinross Gold (2022) | Anchor: Relational (1) + Specific (5) |
| Third Point → Campbell Soup (2018) | Anchor: Hostile (5) + Specific (5) |
| Starboard Value → Huntsman (2019) | Anchor: Relational (1–2) + Broad (1) |
| Ancora/Legion/Macellum → Bed Bath & Beyond (2019) | Anchor: Hostile (5) + Broad (1) |
| Loeb's laudatory Disney letter (2022) | Anchor: low-hostility delivery by a typically confrontational activist |
| Ubben → Hawaiian Electric (2019) | Anchor: high-hostility delivery by a typically friendly activist |

These anchor the endpoints of each 1–5 scale, and the four quadrant cases double as the **calibration set**: before coding any sample, both the LLM pipeline and the human coder must reproduce the author's own classifications. Failure indicates a rubric or prompt defect, not a sample problem — a cheap, decisive sanity check.

## Step 4 — Fill the paper's operational gaps, and label them as researcher decisions

Any real coding effort immediately hits questions the paper does not answer. Each gap was resolved with an explicit choice, flagged in the rubric as "operational decision, not in paper — flag for PI discussion":

- **Multi-demand bundles.** Campaigns typically bundle demands of varying specificity. Decision: code each demand on a sub-scale; campaign-level score follows the most consequential (lead) demand; bimodal bundles get a 3 and a `mixed_demands` flag.
- **When to measure tactics.** Campaigns escalate and de-escalate. Decision: code the campaign *as launched* (initial 13D plus ~30 days of materials), because the propositions concern the tactics activists choose given the PSI configuration at targeting. Later escalation is recorded in a separate trajectory field so it cannot contaminate the launch code.
- **Selection on public filing.** Fully private relational engagement below the 5% ownership threshold never generates a 13D, so the observable sample likely undercounts the relational end of the spectrum. Decision: acknowledge as an inherent selection limitation rather than attempt a fix.

These flagged items are deliberately framed as discussion questions for the author — they mark exactly where his constructs meet operational ambiguity, which is itself a research finding.

## Step 5 — Build in auditability from content-analysis practice

Several rubric requirements come from standard content-analysis methodology rather than the paper itself:

- **Evidence quotes.** Every score must be supported by at least one verbatim quote with document source, making every label auditable.
- **Outcome blinding.** Coders may not use knowledge of a campaign's outcome when coding launch tactics, because outcomes (duration, degree of change, conclusion mode) are the dependent variables for Propositions 6–9; leaking them would bias the test.
- **Validation protocol.** A human-coded blind subsample of 25–30 campaigns, Cohen's kappa on the 2×2 assignments, Spearman correlation on the scales, a taxonomy of disagreements (model error / human error / rubric ambiguity), and a rubric changelog. The changelog doubles as the list of construct-operationalization issues worth raising with the author.

These elements are what distinguish a defensible measurement procedure from ad-hoc model labeling.

## Recommended next steps

1. **Stress-test before building.** Pull Third Point's actual Campbell Soup 13D from SEC EDGAR (cited in the paper) and walk it through the rubric by hand. Rubrics reliably fail on first contact with real documents in unpredictable ways; fix those failures before writing pipeline code.
2. **Promote the rubric to a system prompt.** Once it survives the stress test, the rubric text becomes the Stage-3 classification system prompt nearly verbatim, with the JSON schema as the required structured output.
