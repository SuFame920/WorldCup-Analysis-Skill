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
| 2026-06-22 | norway / senegal | WC2026 GI (MD2) | settled | Norway 46% (不败~84%) | 3–2 (HT 1–0) | 1X2✓ 大小✓ / 让球push 比分✗ 半全✗ | 受让方"favorite不过盘"对(净胜1=push);别默认半场平(哈兰德早段领先) | records/2026-06-22_norway_vs_senegal.md |
| 2026-06-22 | argentina / austria | WC2026 GJ (MD2) | settled | Argentina 70% | 2–0 (HT 1–0) | 1X2✓ 让球✓ 比分✓ 半全✓ / 大小✗ | 校正#2奏效(比分2-0主桶中);但"杀under-bias"过头→精英绞杀被动弱旅其实是低分场 | records/2026-06-22_argentina_vs_austria.md |
| 2026-06-22 | france / iraq | WC2026 GI (MD2) | settled | France 88% | 3–0 (HT 1–0) | 1X2✓ 让球✓ 比分✓ 大小✓ 半全✓ (5/5) | 强弱悬殊场校正#2连中(3-0主桶+吃让2盘);此类场继续信任 | records/2026-06-22_france_vs_iraq.md |
| 2026-06-22 | algeria / jordan | WC2026 GJ (MD2) | settled | Algeria 57% | 2–1 (HT 0–1) | 1X2✓ 比分✓(次桶) / 让球push 大小✗ 半全✗ | 已识别"生死开放局"却押了小球→场景分型没驱动totals;半场别默认平 | records/2026-06-22_algeria_vs_jordan.md |
| 2026-06-23 | portugal / uzbekistan | WC2026 GK (MD2) | settled | Portugal 76% | 5–0 (HT 3–0) | 1X2✓ 让球✓ / 比分✗ 大小✗ 半全✗ | 精英全主力+终结者+动机的强队对真弱旅:比分/totals上限给够(4-0/5-0)、半场别默认平 | records/2026-06-23_portugal_vs_uzbekistan.md |
| 2026-06-23 | england / ghana | WC2026 GL (MD2) | settled | England 76% | 0–0 | 爆冷命中(22%) / 1X2✗ 让球✗ 比分✗ 大小✗ 半全✗ | 自评爆冷≥20%且有具体闷杀机制时,1X2/比分/totals必须对冲(留平/留小),别只写备注 | records/2026-06-23_england_vs_ghana.md |
| 2026-06-23 | panama / croatia | WC2026 GL (MD2) | settled | Croatia 62% | 0–1 (HT 0–0) | 1X2✓ 比分✓(1-0次桶) 大小✓(小) 半全✓(平负) / 让球push | 数据+弱攻对手+押小=最佳工况;净胜1走盘预警精准;但"我方控场"剧本写反(巴控球更多) | records/2026-06-23_panama_vs_croatia.md |
| 2026-06-23 | colombia / dr_congo | WC2026 GK (MD2) | settled | Colombia 60% | 1–0 (HT 0–0) | **5/5 满堂红**(1X2✓ 让球✓-push押对 比分✓1-0 大小✓小 半全✓平胜) | 绞杀铁桶archetype按设计奏效;与Por 5-0同型两极=对手防线质量+终结者决定走向 | records/2026-06-23_colombia_vs_dr_congo.md |
| 2026-06-24 | brazil / scotland | WC2026 GC (MD3) | settled | Brazil 78% | 3–0 (HT 2–0) | 1X2✓ 半全✓(次桶) / 让球✗ 比分✗ 大小✗ | Vini Jr级终结点7'破局→场景②"半场闷"预设被推翻;Raphinha缺席≠巴西进攻降级;比分封顶过低(2-0vs实3-0) | records/2026-06-24_scotland_vs_brazil.md |
| 2026-06-24 | bosnia / qatar | WC2026 GB (MD3) | settled | Bosnia 65% / 平 35% | 3–1 (HT 2–1) | 1X2✅ 让球✅(次桶) 半全✅(次桶) / 比分❌(3-1缺) 大小❌ | 场景③归属正确但比分桶仍保守;卡塔尔残防→应上探3-1;波黑年轻化被低估 | records/2026-06-24_bosnia_vs_qatar.md |
| 2026-06-24 | czech_republic / mexico | WC2026 GA (MD3) | settled | 墨西哥不败(双选) | 3–0 (HT 0–0) | 1X2✅ 半全✅(平负主桶) / 让球❌ 比分❌ 大小❌ | **"轮换≠弱化"**→板凳饥渴反超先发;战意不对等≠弱队有得分价值——此批最贵一课 | records/2026-06-24_czech_republic_vs_mexico.md |
| 2026-06-24 | morocco / haiti | WC2026 GC (MD3) | settled | Morocco 82% | 4–2 (HT 2–2) | 1X2✅ 让球✅(次桶/push) 半全✅(主桶) / 比分❌ 大小❌ | "已淘汰=无威胁"是危险假设;Haiti无压力爆发双球+世界波→totals仅靠"弱攻"推Under脆弱 | records/2026-06-24_morocco_vs_haiti.md |
| 2026-06-24 | switzerland / canada | WC2026 GB (MD3) | settled | 瑞士不败(双选) | 2–1 (HT 0–0) | **1X2✅ 让球✅(次桶/push) 比分✅(2-1!) 大小✅(次桶/3球) 半全✅(平胜主桶)** | BTTS+让平+平胜=均势双出线局"甜点模态";Manzambi精英级变速器;比分量身定做;大小球3球次选命中→5/5满堂红 | records/2026-06-24_switzerland_vs_canada.md |
| 2026-06-24 | south_africa / south_korea | WC2026 GA (MD3) | settled | South Korea 65% | 0–1 (HT 0–0) | 大小✅(1球主桶) / 1X2❌ 让球❌ 比分❌ 半全❌ | "非洲魔咒"不是次要脚注(韩对非洲5场全丢球);核心球员停赛信息误判→方向全错;Under直觉对但方向反 | records/2026-06-24_south_africa_vs_south_korea.md |
| 2026-06-25 | japan / sweden | WC2026 GF (MD3) | pending | 日本胜 52% / 平局 25% | — | — | 非对称战意局(瑞典必须赢/日本打平出线);爆冷~25%→强制双选对冲;场景③开放对攻→Over偏大;久保缺阵→净胜1让平;Isak+Gyökeres个体降维=爆冷主引信 | records/2026-06-25_japan_vs_sweden.md |
| 2026-06-25 | tunisia / netherlands | WC2026 GF (MD3) | pending | 荷兰胜 88% | — | — | 强弱极端悬殊(400-Elo差+突出局+攻防双真空);爆冷~5%→无需对冲;净胜球战意→3-0/4-0;突进攻0威胁→荷兰零封高概率;⚠让球线可能已升-3需核验 | records/2026-06-25_tunisia_vs_netherlands.md |
| 2026-06-25 | australia / paraguay | WC2026 GD (MD3) | pending | 平局 50% / 巴拉圭胜 32% | — | — | 非对称战意局(澳打平出线/巴必须赢);阿尔米隆停赛→巴进攻瘫痪;双方低位→1球/2球极窄区间;盘口平手→走盘概率极高;平平+平胜覆盖 | records/2026-06-25_paraguay_vs_australia.md |
| 2026-06-25 | curacao / cote_divoire | WC2026 GE (MD3) | pending | 科特迪瓦胜 78% / 平局 42% | — | — | 实力断层(200分Elo差)但科特迪瓦终结非精英级(两场仅2球);鲁姆15扑救门神效应→爆冷≥22%触发双选;让2球让胜/让平被低估;MD3溢价totals 2-3球 | records/2026-06-25_curacao_vs_cote_divoire.md |
| 2026-06-25 | ecuador / germany | WC2026 GE (MD3) | pending | 德国胜 50% / 平局 28% | — | — | 动机不对称局(厄必须赢/德已出线);纳格尔斯曼仅2处轮换→非走过场;爆冷~22-25%→强制1X2双选;厄0进球+防线精英→低分闷局;让胜(厄+1)最可能单格 | records/2026-06-25_ecuador_vs_germany.md |
| 2026-06-25 | turkey / usa | WC2026 GD (MD3) | pending | 土耳其不败(平38/土胜35) | — | — | 被动轮换vs全主力死战;美国4核心缺阵+替补门将→防守降级;土耳其xG回归均值(62射0球不可持续);爆冷~40-45%→强制双选;平平+平胜覆盖;竞彩认知差值-28% | records/2026-06-25_turkey_vs_usa.md |
|            |                      |                 |         |                |                 |                                                    |                                                         |                                             |

> **跨场总结(MD2, 2026-06-21,4 场):** 见 [calibration-summary.md](calibration-summary.md)。
> 系统性偏差:**四场全部默认"小球 + 受让方/弱favorite净胜球",→ 大小球 1/4、比分 0/4、让球 2/4**。根因=把单一"强权打不穿摆大巴→低分"叙事套到每场 + 过度外推 MD1 全闷平的小样本。**待办:下次预测前必跑"组合级偏差自检"(见 reflection-protocol)。**

> **跨场总结(MD2, 2026-06-22,4 场):** 见 [calibration-summary.md](calibration-summary.md) Batch 2。
> **大幅好转:1X2 4/4、比分 3/4(上批次 0/4!)、让球 2/4+2push、大小 2/4、半全 2/4。** 校正 #2(不封顶favorite净胜球)**验证有效**(Arg 2-0、Fr 3-0、Alg 2-1 比分主/次桶连中)。残留偏差**已非单向**:① 大小球错向是**场景分型用反**(开放局押了小、绞杀局押了大),不是系统 under-bias;② **新发现"半场平"默认偏差**(Nor/Alg 两场 favorite/对手上半场即领先,半全场 miss)。

> **跨场总结(MD2, 2026-06-23,4 场已结算):** 见 [calibration-summary.md](calibration-summary.md) Batch 3。
> 1X2 **3/4**(England 0-0 爆冷)、让球 Por赢盘✓+Col押push✓/Cro push/Eng✗、大小 **2/4**、比分 **2/4**、半全 **2/4**。**最佳卡=哥伦比亚 5/5(让球直接押 push 命中)+ 克罗地亚 4/5+push。** 两大 miss(Por 5-0、Eng 0-0)主因 **finishing variance vs xG**。两条真·流程缺陷:**① England 机理对却全压热门(没对冲);② Portugal #2 封顶太低 + archetype 误套。** **Por vs Col 同型两极**(见 v3 #10):有数据+真铁桶弱攻对手时近乎满堂红,模型工况最佳。

> **跨场总结(MD3, 2026-06-24,6 场已全部结算):** 见 [calibration-summary.md](calibration-summary.md) Batch 4。
> **1X2 5/6**(仅South Korea 0-1爆冷错)、**让球 3胜+3 push**(push认知稳但Mex/Czech让球完全错向)、**大小 2/6**(SA-KOR 1球✅+Sui 3球次桶✅;赤字来自3场Under→Over误判+Mor 6球井喷)、**比分 3/6**(Sui 2-1精确!但Mor/Bra/Bos上限仍被低估)、**半全 5/6**(最佳项!平胜/胜胜/平负各命中)。**最佳卡=瑞士 5/5满堂红(让平push+比分 2-1+平胜+大小 3球次桶+1X2);最差卡=南非-韩国(信息失误→全错向)。** 三条核心教训:①"轮换=弱化"假设被推翻(板凳饥渴);②"已淘汰=无威胁"假设被推翻;③核心球员信息的准确性是硬地基(F渠道)。

<!--
Append rows like:
| 2026-06-14 | japan / netherlands | WC2026 GF | settled | Japan 48% | 2–2 | ~ | over-rated JPN finishing; NED set-pieces underweighted | records/2026-06-14_japan_vs_netherlands.md |
-->
