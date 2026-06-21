# Memory & Self-Reflection (记忆与自反思)

Turn each prediction into a durable record that (1) **settles itself against reality** the next
time it's relevant, (2) is **retrievable by team and by exact matchup**, and (3) **feeds the
next prediction** as both a calibration correction and a reference prior. This **subsumes the
flat calibration log**: the records live here; `calibration-log.md` is the scoring/diagnosis
*methodology* applied to them.

## Folder layout (memory/ is a DATA folder, parallel to references/)

```
world-cup-analysis/
├── SKILL.md
├── references/        ← how-to (instructions, loaded as needed)
└── memory/            ← data (the persistent record store)
    ├── index.md       ← running table, one row per match (scan this first)
    └── records/
        ├── _TEMPLATE.md
        └── 2026-06-14_japan_vs_netherlands.md   (example of a real record)
```

## Naming & keys (so retrieval actually works)

- **Record file:** `YYYY-MM-DD_<teamA>_vs_<teamB>.md`, team names **canonical English lowercase**,
  in **alphabetical order** (so the exact-matchup key is order-independent:
  `japan_vs_netherlands`, never `netherlands_vs_japan`).
- **Frontmatter `teams:`** carries the canonical pair, e.g. `teams: [japan, netherlands]`. Keep
  any-language display names in the body. The canonical pair is the join key — it avoids
  日本 / Japan / Nippon mismatch.
- **Retrieval modes:**
  - **by team** → any record whose `teams` contains the team (single-team reappearance).
  - **by exact matchup** → record whose `teams` equals the unordered pair (rematch).

## Record schema (per match)

```
---
date: 2026-06-14
teams: [japan, netherlands]          # canonical, alphabetical
display: 日本 vs 荷兰
competition: WC2026 — Group F
venue: Dallas
kickoff_utc: 2026-06-14T20:00Z
status: pending | settled
---

## Prediction (made pre-match)
- overall read: ...
- prior used: market-implied 1X2 at call time (H/D/A)
- 胜平负 / 让球(+线)/ 比分 / 大小球(+线)/ 半全场 — each WITH probabilities
- key swing factors: ...
- divergence from market: ...

## Actual (filled on settlement)
- final: 2–2   | HT: 0–1   | what happened in 2–3 lines

## Reflection (filled on settlement)
- predicted vs actual, per market
- Brier / log-loss per market (see calibration-log.md)
- DIAGNOSIS attributed to a channel:
    news-miss (F) / strength mis-rating (A·C) / tactical misread (E) / pure variance
- one-line reusable lesson

## Transfer notes (what carries forward, with caveats)
- about each team: ...
- about this exact matchup: ...
- context-drift caveats: ...
```

## Lifecycle (the loop) — runs at the edges of the main funnel

**BEFORE recall (consult + settle):**
1. **Lookup.** Scan `memory/index.md` for records involving either team, and for the exact
   matchup. Open the relevant record files.
2. **Lazy-settle.** For any matching record with `status: pending` whose kickoff is now in the
   **past**: web-search the actual result, fill `Actual`, compute `Reflection` (score +
   channel-attributed diagnosis), set `status: settled`, update the `index.md` row. Settlement
   happens **on next use** — no background job needed. If the result can't be verified, leave it
   pending and note that.
3. **Load as Channel H.** Feed the settled reflections into reasoning under the double-counting
   rules below.

**AFTER producing the new prediction (write-back):**
4. **Write a new `pending` record** with the full prediction, and add a row to `index.md`.

## How memory feeds the prediction — and the double-counting guard (the important part)

**Memory's value is the *diagnosis*, not the raw past result.** The old scoreline is almost
always already absorbed by ratings (A) and current form (D); re-importing it as fresh evidence
double-counts and inflates confidence.

- **Single-team reappearance** (a Japan record; next is any Japan match) → contribute a
  **calibration correction**: a *direction + small magnitude* ("last time I over-rated Japan's
  chance creation → apply a small standing haircut"), held as a **weak prior** that only gains
  weight as records accumulate. **Do NOT re-add the scoreline** — the rating already has it.
- **Exact-matchup rematch** (Japan-Netherlands again) → additionally contribute a
  **matchup-specific prior**: how the two styles actually interacted, what surprised the model —
  discounted for **context drift** (knockout vs group stakes, lineup/fitness changes, time
  elapsed). The earlier *result* transfers weakly; the *style-interaction lesson* transfers more.

Emit these into the evidence set as Channel-H records (schema in `evidence-schema.md`): a
calibration-correction record and, for rematches, a matchup-prior record — each with a
`mechanism` and explicitly marked so synthesis treats them as *prior corrections*, not as
fresh outcome evidence.

## Cautions

- **Small sample.** One or two records ≠ a proven bias. Reflections are weak priors; never
  overfit to a single miss. A correct process beaten by variance is not a model error.
- **Attribute honestly.** Only call it a model error if a channel genuinely misread; otherwise
  label it variance. Mis-attributing variance to the model causes overcorrection (chasing noise).
- **Context drift.** Knockout ≠ dead-rubber group game; a rotated XI ≠ the full-strength one.
  Down-weight transfer accordingly.
- **Settlement integrity.** Capture the real final **and** HT score (HT matters for 半全场).

## Relationship to calibration-log.md

`memory/` = storage + retrieval + lifecycle. `calibration-log.md` = the scoring method
(Brier/log-loss) and the cross-record learning ("which channel's adjustments pay"). Apply that
scoring *inside* each record's `Reflection`; read *across* `index.md` for tournament-level
trends (e.g., "my Channel-F injury adjustments add value; my Channel-E matchup leans are noise").
