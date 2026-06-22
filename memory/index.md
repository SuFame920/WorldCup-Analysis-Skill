# Memory Index (索引)

Scan this first on every prediction. One row per match. Look up by either team (single-team
reappearance) or by the exact unordered pair (rematch). Open the matching record file(s) in
`records/`, then settle any `pending` whose kickoff is now in the past.

Status: `pending` = predicted, result not yet recorded · `settled` = result + reflection filled.

| date | teams (canonical) | comp/stage | status | pred 1X2 (top) | actual | hit? | one-line lesson | file |
|---|---|---|---|---|---|---|---|---|
| 2026-06-21 | saudi_arabia / spain | WC2026 GH (MD2) | settled | Spain 82% | 4–0 (HT 3–0) | 1X2✓ 半全✓ / 让球✗ 大小✗ 比分✗ | 精英终结者的短期进球荒会回归——别反买其让球/大球 | records/2026-06-21_saudi_arabia_vs_spain.md |
| 2026-06-21 | belgium / iran | WC2026 GG (MD2) | settled | Belgium 63% | 0–0 | 让球✓ 大小✓ 不败✓ / 1X2主✗ 半全✗ | 过程胜场:结构性哑火(非精英终结者)会延续→低进球/受让方对 | records/2026-06-21_belgium_vs_iran.md |
| 2026-06-21 | cape_verde / uruguay | WC2026 GH (MD2) | settled | Uruguay 62% | 2–2 (HT 2–1) | 让球✓ 不败✓ / 1X2主✗ 大小✗(信心高却错) 比分✗ 半全✗ | 低位弱旅也会进球;别默认"摆大巴=小球" | records/2026-06-21_cape_verde_vs_uruguay.md |
| 2026-06-21 | egypt / new_zealand | WC2026 GG (MD2) | settled | Egypt 56% | 1–3 (HT 1–0 NZ) | 1X2✓ / 让球✗ 大小✗ 比分✗ 半全✗ | 均势局别默认低分;有顶级球员时低估了favorite净胜球 | records/2026-06-21_egypt_vs_new_zealand.md |
| 2026-06-22 | norway / senegal | WC2026 GI (MD2) | pending | Norway 42% (不败~70%) | — | — | 评级倒挂(塞更高)但市场抬挪威靠塞内加尔内乱;让球站受让方 | records/2026-06-22_norway_vs_senegal.md |
|            |                      |                 |         |                |                 |                                                    |                                                         |                                             |
|            |                      |                 |         |                |                 |                                                    |                                                         |                                             |

> **跨场总结(MD2, 2026-06-21,4 场):** 见 [calibration-summary.md](calibration-summary.md)。
> 系统性偏差:**四场全部默认"小球 + 受让方/弱favorite净胜球",→ 大小球 1/4、比分 0/4、让球 2/4**。根因=把单一"强权打不穿摆大巴→低分"叙事套到每场 + 过度外推 MD1 全闷平的小样本。**待办:下次预测前必跑"组合级偏差自检"(见 reflection-protocol)。**

<!--
Append rows like:
| 2026-06-14 | japan / netherlands | WC2026 GF | settled | Japan 48% | 2–2 | ~ | over-rated JPN finishing; NED set-pieces underweighted | records/2026-06-14_japan_vs_netherlands.md |
-->
