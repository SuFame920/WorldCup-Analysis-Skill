# Evidence Schema (定型证据记录)

This is the contract that lets many channels run independently without polluting each other.
**Every channel emits records in this shape. No channel emits a win probability** — that is
synthesis's single, accountable output. Keeping evidence and probability separate is what
makes the whole multi-channel system calibratable and prevents double-counting.

## The record

Each piece of evidence is one record with these fields:

| field | meaning |
|---|---|
| `channel` | which recall channel produced it (A–G) |
| `signal` | the fact, stated plainly (e.g., "Tunisia conceded 5 vs Sweden, xGA ~3.1") |
| `entity` | who/what it's about (team, player, matchup) |
| `direction` | which side it favors, and toward which outcome (e.g., "favors Japan / more goals") |
| `magnitude` | how much it should move things: **negligible / minor / moderate / major** |
| `mechanism` | **required** — *how* it changes the outcome, not just that it correlates. A signal with no stateable mechanism is discarded, not used. |
| `confidence` | how solid the signal itself is: **confirmed / probable / rumor** |
| `source` | where it came from (for tiering — see `sources.md`) |
| `recency` | how fresh (pre-tournament / this week / today / live) |
| `priced_in?` | best guess whether ratings/odds already reflect it (flag for the de-dup stage) |

## Why each field earns its place

- **`mechanism` is the heart.** It is the difference between causal reasoning and
  co-occurrence. "Star striker injured" is not evidence until it says *through what*: injured →
  likely replaced by a lower-output forward → team xG drops ~X → win prob and over-2.5 both
  fall. If you can't write the chain, you don't yet have evidence.
- **`magnitude` + `confidence` are separate axes.** A *major* signal that is only a *rumor*
  must not move the prior like a *confirmed* one. The synthesis stage scales each adjustment by
  both.
- **`priced_in?`** is the double-counting guard. Form that the rating already absorbed should
  not be added again as a fresh nudge. When in doubt, mark it priced-in and let synthesis
  decide.
- **`recency`** lets stale signals be down-weighted or dropped at coarse-rank.

## Worked record (illustrative)

```
channel:     F (pre-match news)
signal:      Japan's first-choice CB carrying a knock, listed "doubt"
entity:      Japan — central defense
direction:   favors Tunisia / slightly more goals for Tunisia
magnitude:   moderate
mechanism:   doubt → possible backup CB → weaker aerial defending → Tunisia's set-piece &
             cross threat (their one goal vs Sweden came from a header) gets a small lift →
             nudge Tunisia goal-prob up, nudge clean-sheet-for-Japan down
confidence:  probable        (not confirmed — "doubt", revisit at lineup release)
source:      tier-2 beat reporter, corroborated by one tier-1 outlet
recency:     today
priced_in?:  no (broke after odds opened)
```

## Aggregation note for player records (Channel C)

Channel C emits *per-player* performance records, then **one team-level aggregate record**.
The aggregate must already have league-normalization and role-context applied, and must be
explicitly labeled a **talent ceiling**, with a pointer that the cohesion discount (Channel E)
is applied later in fine-reason — so synthesis never mistakes the raw ceiling for team
strength.

## What synthesis receives

A clean, de-duplicated list of these records, tier-weighted and recency-filtered. From that
single evidence set it builds the prior, applies bounded adjustments by `direction` ×
`magnitude` × `confidence` × (not `priced_in?`), and emits the **one** posterior distribution
that every bet type is then read from.
