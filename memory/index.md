# Memory Index (索引)

Scan this first on every prediction. One row per match. Look up by either team (single-team
reappearance) or by the exact unordered pair (rematch). Open the matching record file(s) in
`records/`, then settle any `pending` whose kickoff is now in the past.

Status: `pending` = predicted, result not yet recorded · `settled` = result + reflection filled.

| date | teams (canonical) | comp/stage | status | pred 1X2 (top) | actual | hit? | one-line lesson | file |
|---|---|---|---|---|---|---|---|---|
| 2026-06-21 | saudi_arabia / spain | WC2026 GH (MD2) | pending | Spain 82% | — | — | (待结算) | records/2026-06-21_saudi_arabia_vs_spain.md |
| 2026-06-21 | belgium / iran | WC2026 GG (MD2) | pending | Belgium 63% | — | — | (待结算) | records/2026-06-21_belgium_vs_iran.md |
| 2026-06-21 | cape_verde / uruguay | WC2026 GH (MD2) | pending | Uruguay 62% | — | — | (待结算) | records/2026-06-21_cape_verde_vs_uruguay.md |

<!--
Append rows like:
| 2026-06-14 | japan / netherlands | WC2026 GF | settled | Japan 48% | 2–2 | ~ | over-rated JPN finishing; NED set-pieces underweighted | records/2026-06-14_japan_vs_netherlands.md |
-->
