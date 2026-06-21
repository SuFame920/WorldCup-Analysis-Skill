# Sources (信源清单 — 2026, tiered)

Curated places to look, tiered by reliability. The skill retrieves **live** at run time —
this is the map of where to look and how much to trust each, not a cache. Prefer primary/live
sources for anything time-sensitive (lineups, injuries, lines). Re-verify availability
periodically; this landscape shifts.

> **2026 gotcha already baked in:** FBref **lost its Opta licence in Jan 2026** → it is
> **historical-only** now (great archive, but no current-season advanced stats). Don't build
> live analysis on it expecting up-to-date xG.

## Tier 1 — primary / official (trust most for facts)
- **FIFA match centre** — fixtures, kickoff, venue, official lineups when posted, FIFA power
  rankings. Authoritative for schedule and confirmed XI.
- **National federation / club official channels** — confirmed injuries, squad announcements.
- **The Analyst (Opta)** — power rankings, supercomputer probabilities, editorial context
  behind the numbers. Strong analytical baseline.

## Channel-by-channel source map

**A — Team strength (评级)**
- World Football Elo Ratings (eloratings.net) — purpose-built for *national teams*; ideal here.
- Opta / The Analyst power rankings & supercomputer; tournament-specific WC power rankings.
- FIFA world ranking — context only (weak predictor).

**B — Squad value / candidate set (身价)**
- **Transfermarkt** — the only sensible primary for market values, full squad, positions,
  contract/role context. Also injury history.

**C — Player club/league performance (球员联赛数据)**
- **Understat** — xG/xA, shot-level, top-5 European leagues (where many WC players play). Clean
  and free; the workhorse for attacking/finishing quality.
- **StatsBomb open data** — event data, free API, gold-standard model (limited match set).
- **Sofascore / WhoScored** — per-match stats + ratings for leagues the big free sources skip;
  also predicted lineups and match-context visuals. (Opta-sourced; harder to scrape, fine to
  read.)
- **FotMob** — clean live/in-fixture data, mobile-friendly.
- FBref — **historical-only since Jan 2026**; use for past-season context, not current.
- Football-Data.co.uk — downloadable historical results + odds CSVs (for backtesting/modeling).

**D — Form / H2H / schedule (近况/交手/赛程)**
- Sofascore, WhoScored, Soccerway (H2H + fixture/squad history), FBref archive (past seasons).
- For underlying-vs-results, pair scorelines with xG from Understat/Sofascore — a team can lose
  while out-creating the opponent.

**E — Tactical / cohesion (战术/默契)**
- The Analyst (Opta) editorial, reputable tactical writers, match previews (Goal/ESPN/club
  press). Manager tenure & lineup stability from Transfermarkt + news.

**F — Pre-match news / injuries / lineups (赛前新闻) — the NLP channel**
- FIFA + federation/club official (confirmed). Tier-1 outlets (ESPN, BBC Sport, reputable
  national press) for previews and predicted XI. Beat reporters (tier-2) for morale/fitness
  color — corroborate before trusting. **Keep confirmed vs predicted strictly separate.**

**G — Market / odds (盘口)**
- Live odds aggregators / sportsbooks for 1X2, **Asian handicap line**, **goals (O/U) line**,
  and line movement. Forebet for quick algorithmic output (treat as one more signal, not
  gospel). The **line values must be read live** — they are the objects you predict on.

## Tiering rule for the de-dup / coarse-rank stage
Official/primary > tier-1 outlets > tier-2 reporters > aggregators/algorithmic > rumor/social.
A *major* claim from a weak source is downgraded to *probable/rumor* in the evidence schema,
never treated as confirmed. When sources conflict, prefer higher tier + more recent; if
unresolved, keep both and let the distribution widen.

## Reliability reminders
- Cross-check xG across two providers on big calls — models drift on set pieces and wide-angle
  shots.
- Odds/lines move; movement itself is information — note it when visible.
- Be skeptical of single-source "exclusive" injury rumors until corroborated or confirmed.
