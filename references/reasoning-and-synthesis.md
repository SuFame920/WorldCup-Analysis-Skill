# Reasoning & Synthesis (粗排 → 精排 → 重排)

This is where typed evidence becomes one calibrated distribution, and where every bet type is
derived from that *same* distribution. Order matters: clean the evidence, reason causally to
adjust the prior, then synthesize and guard against overconfidence.

---

## 粗排 — Coarse-rank (clean the evidence)

Before anything touches the prior:
- **Dedupe & de-double-count.** Drop or down-weight records marked `priced_in?: yes`. Form
  already in the rating is not a fresh nudge.
- **Recency filter.** Down-weight stale signals; today's lineup news outranks last month's
  form.
- **Source tiering.** Weight by reliability (`sources.md`): official/primary > tier-1 outlets
  > tier-2 reporters > aggregators > rumor. A *major* claim from a weak source is treated as a
  *probable/rumor*, not a fact.
- **Conflict resolution.** When sources disagree, prefer the higher tier and more recent; if
  still unresolved, keep both and let the uncertainty widen the distribution rather than
  picking arbitrarily.

---

## 精排 — Fine-reason (the causal layer)

Adjust the prior **only through stated mechanisms**, and keep each adjustment bounded.

- **Mechanism chains.** For each surviving major/moderate signal, write the chain to outcome
  (the `mechanism` field). No chain → no adjustment.
- **Matchup / style interaction.** Reason about interaction, not addition: press vs build-up,
  low block vs possession, pace vs high line, set-piece threat vs aerial weakness. A stylistic
  mismatch can outweigh a talent gap.
- **Counterfactuals.** For each key availability doubt, reason the "if out" branch explicitly
  and weight it by `confidence` (a doubt is a probability-weighted blend of both branches, not
  a coin flip assumed either way).
- **Cohesion discount.** Apply Channel E's cohesion factor to Channel C's talent ceiling.
  Newly-overhauled or short-camp sides get a larger discount; stable, long-tenure sides little
  to none.
- **Argue the other side.** Before locking a lean, state the best case for the opposite result.
  If it's strong, the distribution should be flatter (more draw / more alternative scorelines).
  This is the explicit antidote to narrative bias.

---

## 重排 — Synthesize (one posterior, then read every bet off it)

### 1. Build the quantitative prior
- Start from rating-implied and **market-implied** 1X2 (Channel A, G). The market is the
  strongest single anchor.
- Convert to expected goals per side (team strength → expected goals, informed by both teams'
  underlying xG for/against from Channel D, adjusted for venue/rest).
- Build a **score matrix** with a bivariate-Poisson-style model over the two expected-goals
  values (allowing a mild correlation/low-score inflation, since real football has slightly
  more draws and 1–0/1–1 than independent Poisson predicts). This matrix is the object every
  bet type is read from.

### 2. Apply bounded adjustments
Move the prior by the fine-reason adjustments, each scaled by `direction × magnitude ×
confidence × (not priced_in)`. **Caps (respect World Cup variance):**
- A single moderate signal: small shift. A single major confirmed signal (e.g., key player
  ruled out): larger, but still bounded.
- **Total** adjustment away from the market prior should stay modest unless evidence is
  overwhelming. Favorites rarely deserve >~65% even after good news; genuinely even matchups
  keep a meaningful draw share (often ~24–30%).
- If your posterior diverges materially from the market, you must be able to name the specific
  mechanism the market is underrating — otherwise pull back toward the market.

### 3. Read each bet type from the posterior

- **胜平负 (1X2):** sum the matrix into Home / Draw / Away. Report 1–2 picks; if the top
  outcome doesn't clearly dominate, the honest answer may be a "双选/draw-inclusive" lean.
- **让球胜负 (Asian handicap):** **look the line up first** (Channel G — 让 0.5? 1? 1.25?).
  Apply the line to the score matrix (shift the favorite's goals by the handicap) and sum which
  side covers, including push/half-win cases for quarter-lines. Predict the covering side. If
  no line is found, predict qualitatively ("若主让一球，倾向…") and say the line is unconfirmed.
- **比分 (correct score):** take the top cells of the matrix. **置信度→个数:** if the top
  scoreline's probability clearly leads (a peaked matrix), give **2**; if several scorelines
  are bunched (flat matrix), give **3**. Tie this to the actual matrix, not gut feel.
- **进球数 / 大小球 (total goals O/U):** **look the line up first** (2.5 / 2.75 …). Sum the
  matrix by total goals and compare to the line. Report 2 picks (e.g., a primary over/under
  lean + a secondary like a band "2–3 球" or the alternate line). Both teams' xG and game-state
  (must-win → open; dead rubber → cagey) drive this.
- **半全场 (HT/FT):** the bet is the **[半场结果][全场结果] pairing**, each ∈ {胜, 平, 负},
  **from the home team's perspective** (the team listed first). That gives **9 outcomes**:
  胜胜 / 胜平 / 胜负 / 平胜 / 平平 / 平负 / 负胜 / 负平 / 负负. The lead-change cases — **负胜**
  (behind at HT, wins FT) and **胜负** (ahead at HT, loses FT) — are the rarest; 平→胜/负 and
  the "持续领先" cases dominate. Build it from a half-aware view: teams score at different rates
  per half and a cautious side may sit 0–0 at the break, so derive per-half scoring rates and
  take the joint distribution over the 9 cells (don't just split the full-time result). This is
  the **highest-variance** market → usually give **1** pick, 2 only when both halves point the
  same way with conviction. Be candid that this is the least reliable line.

### 4. Confidence tagging
Derive a 高/中/低 confidence per pick from how concentrated the relevant slice of the
distribution is (effectively its entropy / the gap to the next option). High confidence ⇒
fewer picks in that category. Low confidence ⇒ say so plainly rather than padding.

### 5. Final guardrails (run before writing)
- **Market sanity check.** Lay your 1X2 next to the market's. Big gap with no named mechanism
  → revise toward the market.
- **Internal consistency.** The bet types must agree: a strong "over 2.5" lean shouldn't sit
  next to a 0–0 top scoreline; a heavy favorite on the handicap shouldn't coexist with a
  draw-leaning 1X2. Reconcile or explain.
- **Overconfidence check.** If everything points one way with no downside noted, you've likely
  fallen for the narrative — re-flatten and re-state the opposing case.

---

## Quick reference: what each market needs from the live web at run time

| Market | Needs a live line? | Read from |
|---|---|---|
| 胜平负 | no (but use market odds as anchor) | matrix → H/D/A |
| 让球 | **yes — the handicap line** | matrix shifted by line |
| 比分 | no | top matrix cells |
| 大小球 | **yes — the goals line** | matrix by total goals |
| 半全场 | no | per-half joint view |

If a required line is missing, predict qualitatively and label it unconfirmed — never invent
the number.
