# Step 2b — Pilot Batch: Selecting a Spread of Campaigns

Companion to `psi_campaign_coding_rubric.md`, `rubric_derivation_method.md`, and the project plan. This note records the reasoning behind the Step 2b pilot batch — why it exists, how campaigns are chosen, and the candidate slate of five.

## Why Step 2b exists (and why we are NOT freezing yet)

Step 2 (the single-filing stress test on Third Point → Campbell Soup) did its job: it threw a real document at the rubric and surfaced a gap, which produced Rule 6 (whole-company sale → specific), Amendment 6a (mocking language as a hostility indicator), and the launch-window timing finding. **Finding a gap and adding Rule 6 is the stress test succeeding, not failing.** A rubric that hit zero problems on its first real filing would be the suspicious outcome.

But one filing surfacing one gap tells us nothing about whether ten more filings would surface ten more gaps. So we do **not** jump to Step 3 (freeze the rubric at v1.0) yet. Step 2b is the real continuation of Step 2, at scale: code a batch of deliberately varied campaigns and keep logging every filing that forces a new rule.

**Stopping rule.** We do not stop when the rubric "works" (unfalsifiable — you can never prove it handles every future filing). We stop when **new rules stop appearing** across a diverse batch. Rule additions should come fast at first, then taper; when a run of structurally varied filings needs *no* new rules, the instrument has stabilized. That asymptote is the signal to freeze (Step 3) — and not before. If rules keep coming, widen the batch.

**Why we freeze at all** (rather than iterating forever): comparability. Once production coding begins (Step 6), changing the rubric mid-run means early- and late-coded campaigns were measured with different rulers and are no longer comparable, which wrecks the theory test. The discipline is: iterate hard and cheaply now, while nothing depends on the results, then lock it.

## Step 2b vs Step 4 — the selection logic is inverted

The two stages look identical (both pull SC 13D Item 4 text off EDGAR) but their sampling goals are opposite:

- **Step 4 (production sample) wants representativeness.** Define a universe (SC 13D + amendments, date range, filters), then pull without cherry-picking — ideally randomly or exhaustively. This is *probability sampling*. Hand-selection here biases the inference and is a fatal flaw. Large N.
- **Step 2b (pilot) wants the opposite — deliberate, adversarial, maximum-diversity selection.** We are not measuring anything yet; we are trying to break the instrument. So we *should* cherry-pick, specifically for weird, varied cases hitting demand types the rubric has not seen. This is *purposive / theoretical sampling*. Non-representative on purpose. Small N.

The mechanics transfer even though the goals don't: the EDGAR-pulling built here is exactly the muscle industrialized in Step 4, so the pilot is not throwaway.

## How the campaigns were picked — five criteria

1. **Exclude the four calibration anchors** (Elliott → Kinross, Third Point → Campbell, Starboard → Huntsman, Ancora/Legion/Macellum → Bed Bath & Beyond) and Campbell (already stress-tested). Those are held out for the Stage 5 calibration set — developing rules on the calibration cases would make calibration circular.
2. **Maximize distinct demand *types*.** Gaps are demand-type-shaped: Rule 6 was a demand-type rule (whole-company sale). So the way to flush out more Rule-6-shaped gaps is to hit demand types never coded before.
3. **Spread across the 2x2 quadrants** (hostility x specificity) so every scale anchor gets exercised across its range, including at least one lower-hostility delivery.
4. **Include a "manner != content" case** — the rubric's hardest rule (code the manner, not the activist's reputation). Achieved by putting the same firm in two opposite postures.
5. **Only reliable SC 13D filers** — activists who actually crossed the 5% threshold on a US target. This criterion excluded some obvious picks (e.g. Trian → DuPont, Icahn → Apple) because those ran *under* 5% via swaps and never filed a 13D — which is itself the selection-on-public-filing limitation the derivation doc already flags.

## Candidate slate (five)

| # | Campaign (approx.) | Demand type — what's new | Quadrant | What it stress-tests |
|---|---|---|---|---|
| 1 | Icahn → Dell, 2013 | Oppose / reprice an in-progress take-private | Hostile + Specific | A demand aimed at a *transaction the company is already doing*, not its operations — outside the rubric's current frame. Strong new-rule candidate. |
| 2 | Starboard → Darden, 2014 | Full board replacement + operational turnaround + real-estate spin | Hostile + *bimodal* | The `mixed_demands` rule head-on (board = broad per Rule 1; the ops plan is hyper-specific → should force a 3 + flag). |
| 3 | Ackman → Target, 2007 | Separate the real estate (REIT) + sell the credit-card unit + buyback | Hostile-moderate + Specific | Financial-engineering / asset-separation — a division/asset demand that tests the *boundary* of Rule 6 (part of the company, not the whole). |
| 4 | Third Point → Yahoo, 2011–12 | Board seats + CEO accountability / removal | Hostile + Broad | Rule 2 (removal = broad *and* hostile) and Rule 3 (removal can still be delivered relationally). |
| 5 | Starboard → Papa John's, 2019 | Negotiated investment + governance stabilization | Relational + mid | "Code the manner, not the activist" — *same firm as #2* in the opposite posture; also exercises the low-hostility anchor. |

Coverage: five distinct demand types (transaction opposition, board-control + ops, asset separation, board + CEO removal, negotiated governance) across four quadrant positions, including one relational.

## Caveats and the immediate next action

- This slate is drawn from training knowledge. Dell, Darden, Yahoo, and Target are fairly confident SC 13D filers (crossed 5%, US targets); **Papa John's needs confirmation** (a convertible-preferred structure complicates the common-stock stake).
- Per the project's provenance discipline, **none of these is "picked" until the actual filing is pulled from EDGAR and confirmed** — no accession numbers or evidence quotes should come from memory. A hallucinated quote would sink the paper.
- **Next action:** verify all five on EDGAR — confirm each filed an SC 13D, capture the accession number and Item 4 text, and swap out any that turn out to be sub-5% or otherwise missing. Then hand-code the batch, logging every filing that forces a new rule. When rule additions dry up across the diverse batch, promote to Step 3 (freeze v1.0).
