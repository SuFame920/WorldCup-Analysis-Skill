# Recall Channels (多路召回)

Each channel is an independent retrieval job. Run them in parallel where possible; each
emits **typed evidence records** (schema in `evidence-schema.md`) — never its own win
probability. Skip a channel cleanly if its data isn't available yet, and record that the
signal is *missing* (absence is itself information: e.g., no confirmed lineup → more
uncertainty).

A channel is worth a full multi-step retrieval only if it needs search + extraction. A
single lookup (one rating, one odds line) is just one fetch, not an elaborate sub-process.

---

## Channel A — Team strength prior (评级)
**Goal:** a base estimate of relative strength, independent of any narrative.
**Pull:** national-team Elo (World Football Elo / eloratings.net), Opta/The Analyst power
rankings & supercomputer probabilities, FIFA ranking (weak — context only). For World Cup,
also tournament-specific power rankings published mid-event.
**Emit:** strength gap, rating-implied 1X2 if available, recent rating trend.
**Watch:** ratings already fold in a lot; don't re-credit the same form again downstream.

## Channel B — Squad value → candidate set (身价名单)
**Goal:** identify *who actually matters*, because the strongest players are not always in
the starting XI and impact can come off the bench or later in the match.
**Pull (Transfermarkt):** the full squad (大名单) with market values; take **at least the top
11 by value including the goalkeeper** as a floor, and extend to ~top 14–16 to capture
rotation and super-subs.
**Emit:** the candidate player set with values and positions.
**Two corrections that matter:**
- **Weight by expected minutes, not raw value.** Value-rank says who *could* matter;
  expected minutes (from Channel F's lineup/rotation signals) says who *will*. The two
  channels feed each other — leave the interface open.
- **Goalkeepers are systematically under-valued** by the market relative to impact; do not
  let value-rank quietly drop or underweight the keeper. Treat GK separately.

## Channel C — Player club/league performance (球员联赛数据) — aggregate up
**Goal:** national-team data is sparse (few competitive matches, mostly qualifiers/friendlies),
so back out a **talent ceiling** from where these players actually play — their clubs/leagues
last season.
**Pull:** for each candidate from Channel B, recent-season club numbers — Understat (xG/xA,
shot-level, top-5 European leagues), Sofascore/WhoScored (ratings + per-match for other
leagues), Transfermarkt (minutes, role). FBref is **historical-only since Jan 2026** — fine
for past-season context, not current.
**Emit:** per-player performance records, then a **team-level aggregate**.
**Three gotchas — do not naively sum:**
1. **League-strength normalization.** A player's Premier League per-90 is not comparable to
   an Eredivisie per-90. Apply a league-difficulty adjustment before aggregating, or the sum
   is garbage.
2. **Role context.** A player's club role may differ from his national-team role (e.g., a
   club No.10 used as a wide midfielder for country). Performance only transfers in the role
   he'll actually play.
3. **Football is NOT additive.** 11 strong individuals ≠ a strong team — especially national
   teams, which have little training time, so automatisms and cohesion matter *more* than at
   clubs. Channel C gives a *ceiling*; it must be discounted by Channel E (cohesion). Don't
   report the raw aggregate as team strength.

## Channel D — Form, H2H & schedule context (近况 / 交手 / 赛程)
**Goal:** recent trajectory and matchup history, with the right discount.
**Pull:** last ~5–6 matches per team (results + underlying xG, not just scorelines — a team
can lose while creating more), head-to-head record, and **schedule effects**: days of rest,
travel, congestion, altitude/heat of the venue. For group-stage matches, capture the
**qualification scenario** (already through / must-win / dead rubber) — this drives rotation
and intensity and is invisible to a flat model.
**Emit:** form trend, underlying-vs-results gap, H2H pattern, rest/travel/venue notes,
group-stage stakes.

## Channel E — Tactical profile & cohesion (战术画像 / 默契)
**Goal:** style matchup and the cohesion discount that Channel C demands.
**Pull/derive:** each team's style (press intensity, possession vs direct, build-up from the
back, set-piece threat/vulnerability), manager tenure, lineup stability, whether the squad
was recently overhauled.
**Emit:** style-matchup notes (does A's press break B's build-up? does B's low block blunt
A's possession?) and a cohesion factor used to discount the talent ceiling.
**Why this beats a flat model:** matchup is interactive, not additive — a mid-tier pressing
side can suffocate a more talented possession side. This is the kind of mechanism the causal
layer reasons over.

## Channel F — Pre-match news: lineup / injury / suspension / motivation (赛前新闻) — the NLP layer
**Goal:** the highest-value, hardest-to-buy signal, mined from text.
**Pull:** predicted vs confirmed XI, injuries and their severity, suspensions, late fitness
doubts, manager press-conference signals, beat-reporter sentiment on morale/form.
**Emit, as structured records:** availability per key player (out / doubt / fit), expected
lineup with confidence, severity-weighted impact, motivation/morale signals.
**Discipline:** keep "confirmed" and "predicted" strictly separate; never upgrade a rumor to a
fact. Severity matters — a fringe player out ≈ noise; a key creator out shifts the whole read.
Feed expected-minutes back to Channels B and C.

## Channel G — Market / odds (盘口)
**Goal:** the strongest baseline and the source of the actual lines to predict against.
**Pull (live):** 1X2 odds → implied probabilities; the **Asian handicap line** (the exact
让球 value — 0.5 / 1 / 1.25 …) and its prices; the **goals line** (大小球 — 2.5 / 2.75 …) and
prices; note any line movement if visible (movement encodes information).
**Emit:** market-implied 1X2, the handicap line + favored side, the totals line, and any
movement.
**Use:** anchor the prior, sanity-check the posterior, and supply the lines the output must
predict on. Beating the market is hard — divergence must be earned and flagged.

---

## Handoff to coarse-rank

Pass the full set of typed records forward. The next stage (粗排, in
`reasoning-and-synthesis.md`) dedupes, recency-filters, weights by source tier, and removes
double-counting **before** any of it touches the prior. Nothing here outputs a probability;
that is synthesis's single job.
