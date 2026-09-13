# Coding Rubric: Activist Campaign Hostility & Demand Specificity

**Source theory:** DesJardine (2025), "The PSI Model for Predicting Activist Engagements," *Academy of Management Review* (Figure 3 activism matrix and surrounding text).

**Purpose:** Classify shareholder activism campaigns on two dimensions — campaign hostility and demand specificity — from public filings (SC 13D Item 4, exhibits, amendments) and linked press materials, to map campaigns onto the four archetypes: specialized supportive, harrowing hardline, gentle general, adaptive aggressive.

---

## 0. Unit of analysis and evidence set

- **Unit:** One campaign = one activist–target pair, beginning at the initial SC 13D filing (or first public engagement if earlier) and ending at exit, settlement, or proxy vote.
- **Evidence documents, in priority order:** (1) SC 13D Item 4 "Purpose of Transaction," (2) exhibits attached to the 13D or amendments (letters to management/board are the richest source), (3) 13D/A amendments, (4) activist press releases, campaign websites, and public letters referenced in filings.
- **Timing rule:** Code the campaign **as launched** — the initial 13D plus materials released within the first 30 days. The theory's propositions concern the tactics activists *choose given the PSI configuration at targeting*, not how campaigns escalate later. Record later escalation/de-escalation in a separate `trajectory` field; do not let it contaminate the launch code.
- **Known limitation (record, don't fix):** Fully private relational engagement below the 5% threshold never generates a 13D. The sample is conditioned on public filing, which likely undercounts the relational end of the spectrum. Flag for discussion; this is a selection issue inherent to the data, acknowledged implicitly in the paper's description of concealed positions.

---

## 1. Dimension A — Campaign Hostility

**Construct definition (from paper):** Hostility is determined by "the nature in which activists make their demands." Hostile campaigns voice criticisms **publicly** and use tactics that **directly attack the company and its management**, sometimes **calling out specific executives and directors and their supposed deficiencies.** Relational campaigns engage executives and directors **privately**, **refrain from calling out personal faults**, and use **neutral language**.

**What hostility is NOT:** It is not the content of the demand. A demand can be aggressive in substance (remove the CEO) but delivered relationally (private engagement, praise for prior service). Code the *manner*, not the *ask*.

### Indicators of hostility (each observed instance raises the score)
- Public criticism of the company's strategy, performance, or governance
- Naming specific executives or directors and attributing deficiencies to them
- Dedicated campaign website criticizing management (e.g., refreshcampbells.com)
- White papers or presentations undermining the company's business approach
- Media appearances or press releases broadcasting criticisms ("public shaming")
- Litigation against the company
- Explicit threats (proxy contest threats framed combatively, ultimatum language)
- Mocking or sarcastic language directed at management/board (e.g., quoting aphorisms to ridicule a governance failure) — added by Amendment 6a, see Changelog

### Indicators of relational engagement (each lowers the score)
- Private outreach as the primary stated engagement channel
- Neutral or technical language when describing problems
- Praise or credit given to management/board (e.g., complimenting strategy shifts, "experienced," "common view")
- Framing of demands as suggestions or shared goals rather than corrections

### Scale (1–5)
| Score | Anchor |
|---|---|
| 1 | Explicitly cooperative: praises management, engages privately, frames ideas as joint value creation. *Anchor: Elliott–Kinross 2022; Loeb's laudatory Disney letter.* |
| 2 | Private-first engagement, neutral tone; public materials factual and impersonal. |
| 3 | Public engagement with criticism of firm performance/strategy, but no personal attacks, no attack apparatus (websites, litigation). |
| 4 | Sustained public criticism of the company AND its leadership as a body (e.g., "abysmal oversight by the board"), threats of proxy contest. |
| 5 | Personal attacks on named individuals, attack websites/videos, litigation, or coordinated media shaming. *Anchors: Third Point–Campbell 2018; Ubben–Hawaiian Electric 2019; Ancora/Legion/Macellum–Bed Bath & Beyond 2019 (lawsuit).* |

**2×2 threshold:** ≥4 → Hostile; ≤2 → Relational; 3 → code but flag `ambiguous_hostility` for manual review.

---

## 2. Dimension B — Demand Specificity

**Construct definition (from paper):** Specificity is "the degree of detail and explicitness with which a shareholder outlines **the entirety of** operational or strategic actions that they want to see executed." Specific demands "mandate the entire set of actions a company should take," limiting managerial discretion and bounding total change. Broad demands "seek overall improvements … without detailing the exact operational or strategic measures," leaving eventual changes undefined and up to management post-engagement.

### Decision rules stated by the author (apply verbatim)
1. **Terminating a named transaction** (e.g., cancel a specific proposed acquisition) → specific.
2. **Capital actions with defined magnitude** (e.g., "repurchase $X of shares," a defined buyback program) → specific.
3. **"General improvements," "refocus the business," "unlock value," "review strategic alternatives"** without stipulated measures → broad.
4. **Board composition changes (adding directors) → broad**, because the directors' eventual strategic/operational impact is undefined at demand time. This is the author's explicit rule; do not override it even when the activist names candidates.
5. **Executive/director removal → broad** on specificity (eventual strategic change undefined) and simultaneously an input to hostility (names a person). Whether it scores hostile depends on delivery manner (Rule in §1).
6. **Demand to sell the entire company → specific (score 4; 5 if a buyer or deal terms are stipulated).** *Researcher-added rule (Amendment 6, see Changelog), not stated in the paper.* Rationale: a whole-company sale bounds "the entirety of" the resulting change — the terminal state (company acquired, independent existence ends) is fully defined, with only counterparty and price left to discretion. This parallels the paper's treatment of named actions with defined scope, and is distinct from demanding a "strategic review" (broad, Rule 3), even when the sale is framed as the review's required outcome.

### Handling multiple demands (operational decision, not in paper — flag for PI discussion)
Campaigns typically bundle demands. Procedure:
- Code **each distinct demand** on a 1–5 specificity sub-scale.
- Campaign-level score = specificity of the **most consequential demand(s)** (those the activist leads with / dedicates most text to), recorded alongside the full per-demand list.
- If the bundle is bimodal (one highly specific + one broad, equally weighted), assign 3 and flag `mixed_demands`.

### Scale (1–5)
| Score | Anchor |
|---|---|
| 1 | Only broad demands: board seats, leadership change, "strategic review," "refocus." *Anchors: Starboard–Huntsman 2019 (board composition); BBB 2019 (leadership changes).* |
| 2 | Broad demands with a named direction but no stipulated actions (e.g., "streamline the portfolio" without naming units). |
| 3 | Mixed bundle, or a named action lacking magnitude/target (e.g., "divest non-core assets" without identifying them). |
| 4 | Named actions with most parameters defined (e.g., "spin off division X"), minor discretion remaining. |
| 5 | Complete stipulated action set: named transactions, quantities, targets. *Anchors: Elliott–Kinross 2022 (defined repurchase program); Third Point–Campbell 2018 (split into two named entities).* |

**2×2 threshold:** ≥4 → Specific; ≤2 → Broad; 3 → flag `ambiguous_specificity`.

---

## 3. Archetype assignment

| | Specific (≥4) | Broad (≤2) |
|---|---|---|
| **Relational (≤2)** | Specialized supportive | Gentle general |
| **Hostile (≥4)** | Harrowing hardline | Adaptive aggressive |

Campaigns with any `3` remain in the dataset with continuous scores but are excluded from archetype counts unless resolved by manual review.

---

## 4. Required output per campaign (LLM and human coders use the identical schema)

```json
{
  "campaign_id": "",
  "activist": "",
  "target": "",
  "filing_date": "",
  "hostility_score": 0,
  "hostility_evidence": ["verbatim quote", "..."],
  "specificity_score": 0,
  "demands": [
    {"demand_text": "", "specificity_sub_score": 0, "author_rule_applied": ""}
  ],
  "specificity_evidence": ["verbatim quote", "..."],
  "archetype": "",
  "flags": ["mixed_demands", "ambiguous_hostility", "..."],
  "trajectory_note": "",
  "confidence": "high | medium | low",
  "reasoning": ""
}
```

Rules for coders (human or model):
- Every score MUST be supported by at least one verbatim quote from the evidence documents, with document source noted.
- If evidence is insufficient to score a dimension, output `confidence: low` and state what document would resolve it — do not guess.
- Do not use knowledge of the campaign's *outcome* when coding launch tactics (outcomes are the dependent variable; leaking them biases the test of Propositions 6–9).

---

## 5. Calibration set

Before coding the sample, both the LLM pipeline and the human coder must correctly reproduce the paper's four worked examples:

| Campaign | Expected code |
|---|---|
| Elliott Management → Kinross Gold (2022) | Relational + Specific → Specialized supportive |
| Third Point → Campbell Soup (2018) | Hostile + Specific → Harrowing hardline |
| Starboard Value → Huntsman (2019) | Relational + Broad → Gentle general |
| Ancora/Legion/Macellum → Bed Bath & Beyond (2019) | Hostile + Broad → Adaptive aggressive |

Failure to reproduce any of these indicates a rubric or prompt defect; revise before proceeding.

---

## 6. Validation protocol

1. Human coder (you) codes a random subsample of 25–30 campaigns using this rubric, blind to model output.
2. Compute agreement: Cohen's kappa on the binary 2×2 assignments; Spearman correlation on the 1–5 scales.
3. Adjudicate every disagreement; classify each as (a) model error, (b) human error, or (c) rubric ambiguity.
4. For category (c), amend the rubric with a new decision rule, log the change, and re-run the affected campaigns.
5. Report: kappa, correlation, disagreement taxonomy, and the rubric changelog. The changelog doubles as a list of construct-operationalization issues to raise with the author.

---

## 7. Changelog

Every rubric change is logged here with its trigger, the decision made, and the open question it raises for the author. Amended rules require re-running any previously coded campaigns they affect.

### Amendment 6 — Whole-company sale demands (2026-07-20)
- **Trigger:** Hand stress-test of Third Point–Campbell Soup SC 13D (filed 2018-08-09). The launch demand — "the only justifiable outcome of the strategic review is for the Issuer to be sold to a strategic buyer" — matched none of the paper's five decision rules.
- **Decision:** Added Rule 6 (§2): whole-company sale → specific (4; 5 with stipulated buyer/terms), reasoning from the construct definition ("the entirety of" actions is bounded by a sale).
- **Question for author:** Is a whole-company sale demand specific under your definition, given that it bounds total change completely but leaves the transaction's parameters (buyer, price) undefined?

### Amendment 6a — Mocking language as hostility indicator (2026-07-20)
- **Trigger:** Same stress test. Third Point's Item 4 mocks the board's succession failure via Ben Franklin's "if you fail to plan, you are planning to fail" — plainly hostile in manner, but not captured by the original indicator list.
- **Decision:** Added mocking/sarcastic language to the hostility indicators (§1).

### Finding (no rule change) — Launch window vs. whole-campaign intensity (2026-07-20)
- **Observation:** The paper anchors Campbell at maximum hostility citing the attack website and video, which were released after the initial 13D — likely outside the 30-day launch window. Coding the launch documents alone yields hostility 4 (not 5): the binary Hostile classification and the harrowing-hardline archetype are preserved, but scale intensity is understated relative to the paper's whole-campaign characterization.
- **Implication:** The 2×2 assignments appear robust to the timing rule; the 1–5 intensities are window-sensitive. Report both, and record post-window escalation in the `trajectory` field.
- **Question for author:** Your propositions concern tactics chosen at targeting — is the launch window the correct measurement point, or should tactic coding aggregate over the full campaign?
