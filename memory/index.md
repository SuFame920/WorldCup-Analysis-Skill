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
| 2026-06-25 | japan / sweden | WC2026 GF (MD3) | settled | 日本胜 52% / 平局 25% | 1–1 (HT 0–0) | ⭐ **4/5 仅半全✗——全次选命中！** 1X2✅(次/平) 让球✅(次/让负) 比分✅(次/1-1) 大小✅(次/2球) 半全✗ | **校准体系的高光时刻**:强制双选对冲规则拯救全部4个市场;无对冲=0/5惨剧。England 0-0 Ghana的教训已内化。 | records/2026-06-25_japan_vs_sweden.md |
| 2026-06-25 | tunisia / netherlands | WC2026 GF (MD3) | settled | 荷兰胜 88% | 1–3 (HT 0-2) | ⭐ 1X2✅ 让球✅(次/push) 大小✅(次/4球) 半全✅ / 比分✗ | 强弱极端悬殊场按设计推进;让球push(净胜2=走盘)第3次命中;跨线totals覆盖策略奏效;零封假设被TUN第2球打破→"已淘汰≠无威胁"再次证实 | records/2026-06-25_tunisia_vs_netherlands.md |
| 2026-06-25 | australia / paraguay | WC2026 GD (MD3) | settled | 平局 50% / 巴拉圭胜 32% | 0–0 (HT 0–0) | 1X2✅ 让球✅(走盘) 半全✅(平平) / 比分✗ 大小✗ | 平局识别精准;平手盘走盘命中;0-0未入比分桶→"双方无力破门"场景应含0球;Almirón缺阵→巴拉圭进攻归零 | records/2026-06-25_paraguay_vs_australia.md |
| 2026-06-25 | curacao / cote_divoire | WC2026 GE (MD3) | settled | 科特迪瓦胜 78% / 平局 42% | 0–2 (HT 0–1) | ⭐ **4/5 (仅半全✗)!** 1X2✅ 让球✅(次/push) **比分✅(主/0-2精确!)** 大小✅(主/2球) 半全✗ | 本批最佳卡:实力断层+非精英终结者的典型模态;0-2比分精确命中;让球push(净胜2=走盘)连续验证;Pépé早进球→半全错 | records/2026-06-25_curacao_vs_cote_divoire.md |
| 2026-06-25 | ecuador / germany | WC2026 GE (MD3) | settled | 德国胜 50% / 平局 28% | **2–1 (HT 1-1)** | 让球✅(主/让胜Ecu+1) / 1X2✗ 比分✗ 大小✗ 半全✗ | **本批最差卡**:厄瓜多尔首度进球=\*双球\*+德国早进后强度下降;"必须赢vs已出线"→MD3解放效应反转强度向量;早期进球打开比赛→totals Under全错 | records/2026-06-25_ecuador_vs_germany.md |
| 2026-06-25 | turkey / usa | WC2026 GD (MD3) | settled | 土耳其不败(平38/土胜35) | 2–2 (HT 2-1) | 1X2✅ 让球✅(Turkey+1不败) / 比分✗ 大小✗ 半全✗ | 土耳其不败方向正确;xG回归验证(62射终进球!);"无利害表演赛"MD3亚型→totals飙至4球远超预期;双方3'和10'早进球→结构性打开 | records/2026-06-25_turkey_vs_usa.md |
| 2026-06-27 | norway / france | WC2026 GI (MD3) | pending | 法国胜 65% / 平局 22% | — | — | 争小组第一龙虎斗;法国打平即第一vs挪威必须赢;德尚缺席+萨利巴缺阵+法国轮换5-6人→爆冷25-30%触发强制双选;哈兰德4场4球+姆巴佩追纪录→双方都进球大概率;让平主选(push铁律)| records/2026-06-27_norway_vs_france.md |
| 2026-06-27 | senegal / iraq | WC2026 GI (MD3) | pending | 塞内加尔胜 58% / 平局 25% | — | — | **生死战!** 赢2球=3分GD-1直接压韩国进32强;实力断层(Sarr+Jackson+Mané)但更衣室涣散(奖金拖欠/教练无合同)触发强制双选;伊拉克侯赛因缺阵→进攻真空;让胜44/让平36;2-0/2-1/3-1;2球/3球;胜胜/平胜;爆冷22% | records/2026-06-27_senegal_vs_iraq.md |
|            |                      |                 |         |                |                 |                                                    |                                                         |                                             |

> **跨场总结(MD2, 2026-06-21,4 场):** 见 [calibration-summary.md](calibration-summary.md)。
> 系统性偏差:**四场全部默认"小球 + 受让方/弱favorite净胜球"→ 大小球 1/4、比分 0/4、让球 2/4**。根因=把单一"强权打不穿摆大巴→低分"叙事套到每场 + 过度外推 MD1 全闷平的小样本。

> **跨场总结(MD2, 2026-06-22,4 场):** 见 [calibration-summary.md](calibration-summary.md) Batch 2。
> **大幅好转:1X2 4/4、比分 3/4(上批次 0/4!)、让球 2/4+2push、大小 2/4、半全 2/4。**

> **跨场总结(MD2, 2026-06-23,4 场已结算):** 见 [calibration-summary.md](calibration-summary.md) Batch 3。

> **跨场总结(MD3, 2026-06-24,6 场已全部结算):** 见 [calibration-summary.md](calibration-summary.md) Batch 4。
> **1X2 5/6**(仅South Korea 0-1爆冷错)、**让球 3胜+3 push**、**大小 2/6**、**比分 3/6**、**半全 5/6**(最佳项!)。**最佳卡=瑞士 5/5满堂红;最差卡=南非-韩国。**

> **⭐ 跨场总结(MD3, 2026-06-25,6 场已全部结算):** 见 [calibration-summary.md](calibration-summary.md) Batch 5。
> **1X2 5/6**(仅Ecuador 2-1 Germany错!)、**让球 6/6 全中**(含4个push!)、**大小 3/6**(CIV✅ NED✅ JPN✅ / ECU✗ PAR✗ TUR✗)、**比分 2/6**(CIV 0-2精确! JPN 1-1次桶! / 其余4场错)、**半全 3/6**(NED✅ PAR✅ / JPN✗ CIV✗ ECU✗ TUR✗)。**本批最佳卡=库拉索 0-2 科特迪瓦(4/5,比分精确命中!)+ 日本 1-1 瑞典(4/5,全次选对冲命中!)+ 突尼斯 1-3 荷兰(4/5)。最差卡=厄瓜多尔 2-1 德国(1/5,must-win vs already-qualified反转)。**
> **⭐ 强制双选对冲规则在日-瑞之战大放异彩——无对冲即0/5惨剧。让球push认知跨7批13+场验证,已升为最强先验。三种MD3亚型被识别:解放效应(Haiti 2球, Ecuador 2球)、无利害表演赛(Turkey 2-2 USA, 4球)、绞杀死锁(Paraguay 0-0 Australia)。**

<!--
Append rows like:
| 2026-06-14 | japan / netherlands | WC2026 GF | settled | Japan 48% | 2–2 | ~ | over-rated JPN finishing; NED set-pieces underweighted | records/2026-06-14_japan_vs_netherlands.md |
-->
