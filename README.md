# activism-matrix

**Does a new theory of shareholder activism hold up against real SEC filings?**

DesJardine (2025, *Academy of Management Review*) proposes that activist investors' campaign tactics vary along two dimensions — **campaign hostility** (relational ↔ hostile) and **demand specificity** (broad ↔ specific) — producing four campaign archetypes, and predicts which conditions produce which archetype. The theory is new and untested.

This project operationalizes that "activism matrix" and runs the first descriptive pilot against real campaigns: SC 13D filings pulled from SEC EDGAR, classified by an LLM pipeline against a formal coding rubric, and validated against blind human coding.

> **Attribution:** The matrix, its dimensions, archetypes, and propositions are DesJardine's theoretical contribution. This repo contributes the operationalization (coding rubric), the measurement pipeline, the coded dataset, and the pilot findings — including documentation of where the constructs are operationally ambiguous.

## The matrix being tested

|  | Specific demands | Broad demands |
|---|---|---|
| **Relational tactics** | Specialized supportive | Gentle general |
| **Hostile tactics** | Harrowing hardline | Adaptive aggressive |

Each campaign is scored 1–5 on both dimensions from its launch materials (initial 13D, Item 4, exhibits, first ~30 days), with verbatim evidence quotes required for every score.

## Pipeline

```
EDGAR full-text search ──► SC 13D corpus (Item 4 + exhibits + 13D/A amendments)
        │
        ▼
Passage retrieval (embeddings + rerank) ──► demand- and tone-relevant excerpts
        │
        ▼
LLM classification (rubric as system prompt, structured JSON output,
temperature 0, dual runs, disagreements flagged)
        │
        ▼
Calibration gate: must reproduce the paper's 4 worked cases
(Kinross, Campbell Soup, Huntsman, Bed Bath & Beyond)
        │
        ▼
Human validation: blind hand-coding of 25–30 campaigns,
Cohen's κ (2×2) + Spearman ρ (scales), disagreement taxonomy
        │
        ▼
Outcomes join (duration, conclusion mode) + descriptive analysis
vs. Propositions 6–9
```

## Repo contents

- `docs/psi_campaign_coding_rubric.md` — the coding rubric: construct definitions, decision rules, 1–5 anchored scales, output schema, calibration set, validation protocol
- `docs/rubric_derivation_method.md` — how the rubric was derived from the paper, with researcher decisions explicitly separated from the author's rules
- `ingestion/` — EDGAR scraping and filing parsers *(in progress)*
- `pipeline/` — retrieval + classification *(in progress)*
- `analysis/` — validation stats and matrix distribution *(planned)*

## Method notes

- **Timing rule:** campaigns are coded *as launched*, because the theory's propositions concern tactics chosen at targeting; later escalation is tracked separately.
- **Outcome blinding:** coders (human and model) never see campaign outcomes when coding tactics — outcomes are the dependent variable.
- **Every label is auditable:** scores without verbatim supporting quotes are rejected.

## Honest limitations

- **Descriptive, not causal.** The PSI conditions aren't observed independently of the campaigns; this is a pilot of the matrix as a measurement framework, not a causal test of the propositions.
- **Selection on public filing.** Fully private engagement below the 5% ownership threshold never generates a 13D, so the sample undercounts the relational end of the spectrum.

## Status

🚧 Early stage — rubric complete, EDGAR ingestion in progress. First target: pass the calibration gate on the paper's four worked cases.

## Reference

DesJardine, M. R. (2025). The PSI model for predicting activist engagements: Toward a theory of shareholder activism. *Academy of Management Review*. https://doi.org/10.5465/amr.2024.0006
