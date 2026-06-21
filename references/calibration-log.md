# Calibration Log (校准日志)

Without this, the whole system is unfalsifiable storytelling. With it, you learn — over a
tournament — whether your adjustments add signal or just noise. You can't learn channel
weights from data here (sample is tiny), so **this log is your only feedback channel** — the
closest thing to "training the ranker." Build it from the first prediction; don't bolt it on
later.

## Log one row per prediction, with probabilities (not just the pick)

Scoring needs the **probability you assigned**, not only the chosen outcome. Record:

| field | example |
|---|---|
| `match` | Tunisia vs Japan (WC2026 GF) |
| `date` | 2026-06-21 |
| `kickoff_info_state` | lineups predicted / confirmed; line available? |
| `prior_1x2` | market-implied at time of call (H/D/A) |
| `posterior_1x2` | your H/D/A probabilities |
| `score_dist_top` | top scorelines + probs |
| `handicap_line` + `pick` + `p` | Japan -1 / Tunisia +1 / p=0.55 |
| `totals_line` + `pick` + `p` | O/U 2.5 / under / p=0.58 |
| `htft_pick` + `p` | Draw/Japan / p=0.30 |
| `key_swing_factors` | Kubo & Kamada starting; early goal |
| `divergence_from_market` | where & why you differed |
| `result` | filled after the match |
| `scores` | Brier + log-loss per market (after result) |

## Scoring (after the result)

- **Brier score** (lower better) for each probabilistic market — the standard calibration
  metric. For 1X2: Brier = Σ over {H,D,A} of (p_outcome − actual)², where actual is 1 for the
  result that happened and 0 otherwise.
- **Log-loss** (lower better) as a sharper penalty for confident misses: −ln(p assigned to the
  actual outcome). One overconfident wrong call hurts a lot — exactly the signal you want
  against narrative bias.
- Track per **market type** separately. 半全场 will (correctly) score worse than 胜平负 —
  that's expected; what matters is each market's trend over many matches.

## How to actually use it (the point)

1. **Beat the market, or admit you don't.** The key test isn't "was I right" but "did my
   posterior beat the market-implied prior on log-loss, over many matches?" If not, anchor
   harder to the market and adjust less.
2. **Find which channel's adjustments pay.** When a call beats/misses the market, note which
   adjustment drove the divergence. Over a tournament, patterns emerge — e.g., "my injury-news
   adjustments add value; my tactical-matchup leans are noise." That tells you which recall
   channels to trust and which to down-weight. This is the manual substitute for learned
   weights.
3. **Watch for systematic miscalibration.** If outcomes you call "70%" happen ~50% of the time,
   you're overconfident across the board → widen distributions / tighten the adjustment caps in
   `reasoning-and-synthesis.md`.
4. **Never relitigate with hindsight.** Score against the probabilities you actually assigned
   *before* kickoff. A correct process that lost to variance is not a bad process; resist
   rewriting the call after the fact.

## Storage

A simple append-only table (CSV/Markdown) is enough. The discipline — logging probabilities
every time and scoring honestly — matters far more than the tooling.
