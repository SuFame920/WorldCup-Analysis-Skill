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

**⚠️ 独立预测再比市场(2026-06-28 P0 新增 —— 必须执行，不可跳过):**
精排阶段要先独立判断，再与市场对比。市场作为锚而非驱动力。

1. **先用 A-F 推导「模型概率」(不看 G / 赔率)。** 基于评级(A)、身价(B)、联赛数据(C)、近况(H2H/D)、战术对位(E)、赛前新闻(F)推导出你自己的 1X2 概率分布和期望进球数。这步的目标是**形成独立判断**，而不是为赔率找叙事支撑。
   对于联赛比赛：A 的评级可来自联赛积分榜+近期 xG 差而非仅国家队 Elo；D 的重点是主客场分拆（有些队主场/客场判若两队）；C 的数据是完整赛季数据而非杯赛小样本。
2. **再读 G(赔率隐含概率)。** 获取市场 1X2 隐含概率、让球线、大小球线。
3. **对比模型 vs 市场:差距 > 8% 才算独立信号。**
   - 差距 ≤ 8% → 锚定市场概率，显式写"与市场一致，无独立偏离"
   - 差距 > 8% → 必须能**点名具体机制**解释差异(如"市场低估 XX 的早进球能力")，且受 cap 约束
4. **写出差异分析:** "我与市场的差异在 ___，原因 ___，对应机制 ___"。如果无差异，写"与市场方向一致，确认无独立偏离"即可，但不降低到"盲从"——你仍然需要用自己的话解释比赛逻辑。
5. **偏离幅度受 cap 约束:** favorite ≤~65%(均势 50–55%)，平局 24–30%。即使有独立信号，不能无限偏离市场。

**对均势局的直接影响:** 均势局市场本身无显著倾向 → 模型步骤 1 也应输出扁平分布 → 步骤 3 差距小 → 步骤 4 "无实质分歧" → 输出用双选/三选，降低单选置信度上限。

---

**⚠️ 均势局专用行为规则(2026-06-28 P1-b 新增 —— 强制触发):**
当 **市场 H/A 隐含概率差距 < 20%** 或 **ELO 差 < 100** 或 **双方均无精英终结者** 中任一条件满足时,判定为**均势局**，执行以下规则：

**触发条件（三选一即触发）：**
1. 市场隐含概率 |P(H) − P(A)| < 20%（赔率显示双方胶着）
2. ELO 差 < 100（实力相近）
3. 双方均无顶级终结者（xG/90 > 0.5 的射手；即无人能单点打破僵局）

**均势局规则(强制，不可豁免)：**

| 市场 | 规则 | 原因 |
|---|---|---|
| **1X2** | **不出单选**。必须双选(胜+平，若市场倾向 H)或三选(若市场完全无倾向)。置信度上限 = 50。 | 均势局市场本身无倾向，不存在独立信号 |
| **让球** | 重心转向**走盘和受让方有值**。整数盘走盘为主选(~35–40%)；次选受让方赢盘(而不是强方穿盘)。 | 均势局净胜 > 1 的概率极低 |
| **比分** | 必须覆盖**三个格**:低分胜(1-0)、平(1-1/0-0)、高胜(2-1)。**不按强弱方向聚集**。 | 胜负只在毫厘之间，比分必须覆盖双向 |
| **大小球** | 用 **E[goals] = (xG_A + xG_B) × 场景系数** 锚定(见重排 §0)，**不与"谁更强"挂钩**(谁更强在均势局无意义)。Under/Over 均可，只由 xG 数据驱动。 | 去掉"强队=进球、弱队=防守"的方向偏差 |
| **半全场** | 平平(40–50%)为最强先验；次选给"受让方领先半场+强方反扑"的负胜或平胜。置信上限 = 40。 | 均势局半场最可能是 0-0 |

**在输出中标注：** 整体方向分析中写明"**[均势局]**：双方实力/赔率接近，以下为概率分布而非确定性预测。"

---

### 比赛类型自适应(2026-06-28 P3 新增 · 联邦学习之于所有市场)

在精排阶段**自动检测比赛类型**，以下判断影响所有下游市场：

| 维度 | 联赛常规 | 杯赛小组 | 杯赛淘汰 | 友谊赛 |
|---|---|---|---|---|
| **主场权重** | 高(+0.3 E[goals]偏移) | 中立 | 中立/主场混合 | 中立/无意义 |
| **动机** | 表压力+保级/欧战 | 出线+轮换+死签不对称 | 双方必须赢(对称) | 无压力(高轮换) |
| **冷门概率** | 低(15–20%) | 中(20–30%) | 中高(25–35%) | 高(30%+，无意义) |
| **大胜概率** | 低(EPL/LaLiga顶级联赛很少5-0) | 中(对弱旅可穿盘) | 低(GD封顶~2) | 高(弱队放弃防守) |
| **轮换风险** | 低(除非赛程密集) | 高(末轮尤其+已出线/已淘汰) | 极低(全主力) | 极高(教练试验阵容) |
| **数据适用** | 赛季数据(大样本可靠) | 赛事内数据(小样本) | 赛事内数据+过往淘汰赛 | 不可靠(忽略) |
| **F渠道首发** | 可在kickoff前30min获知 | 必须kickoff前1h搜索 | 必须kickoff前1h搜索 | 不必过度投入(意义低) |

**联赛特有因子(新增):**
- **主客场拆分:** 联赛中有些队主场/客场判若两队——Channel D 的"近期 5 场"应将主客场拆开分析，不能混在一起取均值
- **圣诞/密集赛程:** 英超 12 月-1 月、各国杯赛周中→轮换概率↑、体能↓→E[goals]调低 0.2
- **德比战:** 同城/宿敌德比→犯规率↑、红牌概率↑、进球通常偏低(紧张+谨慎)
- **赛季末无意义场:** 双方均已保级/无欧战希望→等于友谊赛

**检测方式(运行时):** 从用户输入自动识别——提及"世界杯/欧洲杯/亚洲杯"→大赛杯赛；提及"英超/西甲/德甲"→联赛；提及"淘汰赛/R16/八强/半决赛/决赛"→杯赛淘汰；提及"友谊赛/热身赛"→友谊赛。不确定时主动问用户一句"这是联赛还是杯赛？什么阶段？"

---

- **长链因果推演(资深球迷视角,用户定制 ①).** 别停在一阶"谁强谁赢"。把链条推到二、三阶:
  强在哪 → 对手如何针对 → 反制 → 临场催化剂(早丢球 / 红牌 / 换人 / 天气)把比赛导向何处。
  数据(评分 / xG / 盘口)是**锚与验算,不是拐杖**;数据与足球逻辑冲突时,说清你更信哪条链、为什么。
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
  to none. **Adapt to team type**: club teams (daily training) get a smaller discount than
  national teams (occasional camps); in-league matches (familiar opponents) get a smaller
  discount than inter-league or international matchups.
- **Argue the other side.** Before locking a lean, state the best case for the opposite result.
  If it's strong, the distribution should be flatter (more draw / more alternative scorelines).
  This is the explicit antidote to narrative bias. **这一步的产物在重排阶段汇成「爆冷概率」(见 3.5),
  而不是写成输出端的犹豫措辞。**
- **反矫枉过正自检(anti-overcorrection,2026-06-22 批次教训).** 载入 `memory/corrections.md`
  的 standing corrections(Channel H)作为弱先验时,每应用一条先问:**"我是不是在把上一批次的教训矫枉
  过正?"** 修正是**带方向+小幅度+帽子的弱先验,不是反向铁律**:上次押小错了**不等于**这次无条件押大
  (本批 Arg-Aut 正因把"杀 under-bias"用成"一律押大"而错)。优先用**条件式修正**(如 totals 按场景
  分型)而非无条件标量;一条修正与本场具体机制冲突时,信本场机制,别硬套历史修正。

---

## 重排 — Synthesize (one posterior, then read every bet off it)

### 0. E[goals] 锚定 —— 大小球的定量脊柱(2026-06-28 P1 新增)

> **This section must be executed before the Poisson matrix, before any totals pick.**
> The E[goals] number is the **anchor**; narrative adjustments shift it within caps, but the
> anchor itself is quantitative. This replaces the old "narrative-first, then check xG" approach.

#### 0.1 数据源与公式

从 Channel D 的近况数据取两队最近 5 场所有比赛 xG_for/against 的**均值**（加权：时效靠隔~20%）。
**比赛类型自动检测**：世界大赛（世界杯/欧洲杯等）×1.5（强度更高，xG 真实反映能力）；普通联赛 ×1.0。

```
xG_A = mean of team A's last 5 match xG_for (weighted: more recent ×1.2, major tournament ×1.5)
xG_B = mean of team B's last 5 match xG_for (same weighting)
E[goals] = (xG_A + xG_B) × 联赛/杯赛修正 × 场景系数
```

**联赛/杯赛修正(新增)**：
- 联赛常规：×1.0（主客场对称，双方互相熟悉，防守效率高 → 进球偏少）
- 杯赛小组：×1.0
- 杯赛淘汰：×0.88（已内置在场景系数）
- 友谊赛：×1.05（强度低，防守更松散）

**如果 Channel D 的 xG 数据缺失**(小国/弱联赛/低级别联赛常见)：
- **国家队**：用 Elo 差推算：`E[goals] ≈ 2.55 + 0.005×(Elo_A − Elo_B)` （全球均值 ~2.55 球/场，每 100 Elo 进球预期差 ~0.5）
- **俱乐部联赛**：用联赛均值替代——查阅该联赛本赛季场均进球数，用它与两队积分差做锚定（如英超 ~2.85、西甲 ~2.55、意甲 ~2.65、德甲 ~3.05、法甲 ~2.75）；积分差每 10 分调整 ~0.15 球
- 标注 "xG 缺失，用联赛/赛事均值推算，精度降级"，E[goals] 的置信度封顶 ~40

#### 0.2 场景系数表(2026-06-28 终版)

| 场景判别键 | 系数 | 解释 |
|---|---|---|
| **真鱼腩屠戮** (精英全主力+顶级终结者 vs ELO差>400+防线崩溃的对手) | ×1.35 | 历史均值~3.5+球；上探 4-0/5-0 |
| **开放对攻** (双方都必须赢/缠斗+互有得分点+无铁桶) | ×1.10 | 2.5~3.5 球为最可能区间 |
| **表演赛型** (友谊赛/锁定排名的末轮) | ×1.05 | 开放但无压力，略高于均值 |
| **常规中立场**(基准) | ×1.00 | 无特殊动机/打法的普通对抗 |
| **强控场弱攻** (一方精英控场绞杀+对手进攻贫弱/无追分能力) | ×0.90 | 2 球以下为主 |
| **淘汰赛常规**(新增) | ×0.88 | 双方都强+谨慎优先 |
| **绞杀死锁** (双方铁桶+无动机进攻) | ×0.75 | **必须含 0-0 比分桶**；0~1 球为主 |
| **退役大巴型** (一方全力铁桶+无反击意愿) | ×0.65 | 极少见；0 球主导 |

**判别流程(按顺序，不要跳):**
1. 双方是否都是铁桶/无动机进攻？ → YES → 场景⑥/⑦ → 系数 0.65–0.75
2. 是否强弱悬殊(ELO 差 > 400 且有顶级终结者)？ → YES → 场景① → 系数 1.35
3. 是否双方都必须赢/缠斗(淘汰赛/出线生死)？ → YES → 场景②(×1.10 对攻)或③(含平局)
4. 是否淘汰赛？ → YES → 系数 0.88 起步,叠加上面判断
5. 默认 → 基准 ×1.00

#### 0.3 强制规则(硬约束)

- **E[goals] < 1.5 → 必须写双理由(防守数据 + 动机都指向极低进球)，缺一不可。** 单理由(如"沙特必须赢所以 0-0")→不成立。
- **E[goals] > 4.0 → 必须写双理由(进攻数据 + 防守漏洞都指向进球潮)，缺一不可。**
- **"双方打平即可" ≠ 低进球**：判别键是防守能力，不是进攻动机（见 corrections.md #10）。
- **E[goals] 数字在大小球子分析中显式写出**。
- **0-0 桶强制触发**：平局入次选 + 铁桶机制 → 0-0 必须入桶（见 corrections.md #17）。

#### 0.4 从 E[goals] 到 picks

| E[goals] 范围 | 主选总进球 | 次选 |
|---|---|---|
| ≤1.0 | 0 球 | 1 球 |
| 1.0–1.8 | 1 球 | 0 球 或 2 球 |
| 1.8–2.5 | 2 球 | 1 球 或 3 球 |
| 2.5–3.3 | 3 球 | 2 球 或 4 球 |
| 3.3–4.0 | 4 球 | 3 球 或 5 球 |
| >4.0 | 5 球 | 4 球 或 6 球 |

**Picks 必须与比分自洽**：你给的比分总进球要正好落在你给的大小球进球数上(如押 2 球/3 球 → 比分用 1-0, 2-0, 2-1 这类，禁止 1-1=2 球却押 3 球)。

#### 0.5 实时调整(比赛进行中的修正)

若 F 渠道提供开场 30 分钟内的关键事件（红牌、早进球、核心伤退），重新计算：`E[goals]_updated = E[goals]_pre + 调整量`，调整量见 corrections.md #11 #12。

---

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

> **果断,不兜底(用户定制 ④).** 每项**直接给预测**。表达不确定性只许通过:选项个数
> (`置信度→个数`)、置信度标签、独立的爆冷概率(3.5)——**禁止在预测后追加"但 X 也很活/也可能"
> 之类自我拆台**。一个"双选 / 含平"本身是一个**果断的选择**(不是兜底);兜底指的是给完单选后又
> 偷偷补一个反向选项。另外:**说人话**,术语第一次出现给白话解释(见 output-format)。

**先消费"爆冷机制"再落选项(2026-06-23 批次教训 —— England 0-0).** 读每个市场之前,先取
精排「argue the other side」与 3.5 得到的**爆冷机制**作为*输入*,别等选项给完了才事后报个数。
爆冷不是装饰性数字——它必须塑造 **totals 的分布形状、半全场的半场权重、1X2 的选项个数**(见各
市场下的"爆冷钩子"与 3.5 的绑定规则)。**England 反例**:22% 爆冷 +「低位铁桶闷成 0-0」的机制写
进了备注,却把 totals 押了偏大、1X2 单押热门——机制没回流到选项,五项随之全错。

- **胜平负 (1X2):** sum the matrix into Home / Draw / Away. Report **≤2** picks; if the top
  outcome doesn't clearly dominate, the honest answer may be a "双选/draw-inclusive" lean. **这个
  方向一旦定下,就锁死了比分、半全场、让球的方向**(见 step 5 的不矛盾硬约束)。
  - **爆冷钩子(对冲走双选通道).** 当**爆冷概率 ≥~28–30% 且有具体机制**时,优先把 1X2 做成
    **双选(含平 / 1X)**——这是把对冲**合规放出去**的正道(双选会合法地打开比分取平局格、半全场
    取平),而不是单押热门后偷偷补反向选项(那违反不矛盾硬约束)。若爆冷虽高但平局份额不足、机制
    偏"输球"而非"逼平",1X2 可仍单押,但 totals / 半全场 仍按下面的钩子向爆冷尾部配重。
- **让球胜负 (体彩整数让球):** **look the line up first** (Channel G — 体彩竞彩是**整数盘**,如
  让 1 / 受让 1)。把整数让球套到比分矩阵(给受让方加上让球数),再汇总**让球后的胜/平/负**(主队
  视角)。预测让球后最可能的那一面。**用体彩口径说"让 N 球后胜/平/负",不要用 -0.25/-0.5/1.25 这类
  亚洲盘四分之一/半球术语。** 若线查不到,定性预测("若主让一球,倾向…")并注明线未确认。Report **≤2**。
  - **整数盘"走盘(push)"必须显式处理(2026-06-22 批次教训).** 体彩是整数盘:favorite 让 1 而**净胜恰好 1 球**=**走盘退款**,既非赢盘也非输盘。所以**别把"favorite 赢不下让球盘"自动等价于"受让方净赢"**——"净胜 1"落在 push 这一格。读矩阵时把 push 概率单列出来,受让方真实赢盘 value 只算 favorite 净胜 0(平/负)的质量,不含 push。本批次 Nor 3-2、Alg 2-1 两场 favorite 均净胜 1=push,验证此坑。
- **比分 (correct score):** take the top cells of the matrix **that agree with your 1X2 pick**.
  押主胜→只取主胜比分;押双选(胜+平)→只取主胜或平局比分,**禁止取客胜比分**。**置信度→个数:**
  头部明显领先→给 1–2 个;多个挤在一起→给 **3**(上限)。按概率从高到低排列,高的在前。
- **进球数 / 大小球 (total goals O/U):** **look the line up first** (2.5 / 2.75 …) 用于核对方向,
  但**预测写成具体进球总数**(如"2 球 / 3 球"),不要写"大球/3+"这种开口区间。Sum the matrix by
  total goals, take the **≤2** most likely totals. **这些进球数必须与你列的比分总和一致**(2-1=3 球)。
  - **场景分型决定 totals 方向(2026-06-22 批次教训 —— 别用无条件标量).** 不要笼统"押大/押小",按局势分型,且**真正让分型驱动数字**(本批次两次错都是分型用反):
    - **开放对攻局**(双方都必须赢 / 出线缠斗 / 互有得分点)→ **偏大,贴市场上沿**。例:Nor-Sen 5 球、Alg-Jor 3 球(我赛前已写"生死必开放"却仍押小=分型没落地)。
    - **精英 favorite 控场绞杀被动弱旅**(对手摆大巴+运动战钝+无追分动机/能力)→ **可低分,贴市场下沿甚至偏下**。例:Arg-Aut 2-0(我误把"杀 under-bias"当成"一律押大"→押大错)。
    - **判别键**:对手有无**追分动机 + 追分能力**;favorite 是**对攻**还是**控场绞杀**。先判分型,再读矩阵取进球数。
  - **爆冷钩子 + 置信封顶(2026-06-23 批次教训,必做).** 若爆冷主要由**低分闷杀机制**(铁桶+缺开锁手 / 控球哑火 / 门将封神)驱动,则 totals 是**双峰分布**(要么强队凿穿打 2–3,要么 0-0/1-0 磨死)——**under 尾部必须配重**,别因"favorite 强 / 后防漏"就反射性押偏大(England 押大→0 球的根因)。**精英强弱场的 totals 方差经三批验证极大 → totals 置信度封顶 ~45**,把握不足时**横跨盘口线**(留大+留小各一)而非站定一边。
- **半全场 (HT/FT):** the bet is the **[半场结果][全场结果] pairing**, each ∈ {胜, 平, 负},
  **from the home team's perspective** (the team listed first). That gives **9 outcomes**:
  胜胜 / 胜平 / 胜负 / 平胜 / 平平 / 平负 / 负胜 / 负平 / 负负. The lead-change cases — **负胜**
  (behind at HT, wins FT) and **胜负** (ahead at HT, loses FT) — are the rarest; 平→胜/负 and
  the "持续领先" cases dominate. Build it from a half-aware view: teams score at different rates
  per half and a cautious side may sit 0–0 at the break, so derive per-half scoring rates and
  take the joint distribution over the 9 cells (don't just split the full-time result). This is
  the **highest-variance** market → usually give **1** pick, **≤2** only when both halves point
  the same way with conviction. **全场段必须与 1X2 一致**(押主胜→全场段=胜)。Be candid that this
  is the least reliable line.
  - **别反射性默认"半场平"(2026-06-22 批次教训).** 当一方有**早段破僵能力**时,**上调其半场领先概率**,半全场别默认押"平/X":① 精英早段终结点(哈兰德/姆巴佩级,本批 Nor 上半场即领先);② 对手快反/定位球能先偷一城(本批 Alg-Jor 约旦上半场 0-1 领先)。本批次 Nor、Alg 两场半全场都错在 HT-平默认。仅当**双方都谨慎、缺早段得分点**时才回到"半场平"先验。
  - **爆冷钩子 + 置信封顶(2026-06-23 批次教训).** 当爆冷由"对手深蹲铁桶"驱动时,**0-0 半场**(进而 平 / 平负 / 平胜)要给权重,别因押了强队全场胜就默认半场也领先(Por 押平胜实胜胜、Eng 押胜胜实平平=两向都错 HT)。半全场是**最高方差市场**,**置信度封顶更低(~40)**,通常只给 1 个。

### 3.5 爆冷概率(upset probability)— 必给的独立产出(用户定制 ②)

爆冷 = 比赛出现**跌破普遍预期**的结果。**它不等于弱队(平 + 胜)概率的简单相加**——那只是个起点参照,
要在其上做**整体、因果**的上下调整。这是一个**独立**的数字,不是从 1X2 直接抄来的。

1. **先界定"普遍预期"是什么.** 不只看谁赢,还看赢多少、怎么赢(盘口 + 大众叙事)。算"冷"的包括:
   强队被逼平、爆冷输球、**强队赢球却赢不下让球盘**、控球强队被 0–0 闷死。
2. **从弱队 (平+胜) 概率起步,然后按机制调整(不是统计巧合,要有因果链):**
   - **上调**:强队已提前出线 / 赛程疲劳 → 轮换、松懈;动机失衡(弱队生死战 vs 强队练兵);
     打法被克制(铁桶 + 反击克"控球但哑火"的强队);**高方差催化剂**——对方门将封神、红牌、点球、
     过度依赖定位球、极端天气 / 草皮、长途奔波、大胜之后的麻痹。
   - **下调**:强队需要分数全力出击、主力齐整状态在线;弱队伤停 / 已出局摆烂;风格相性利于强队。
3. **产出**:一个**百分比或 低/中/高**,并点名**最可能引爆的 1–2 个机制**。
4. **与 1X2 的关系**:视角不同——1X2 回答"最可能是谁赢";爆冷概率回答"普遍判断这次有多大概率被打脸"。
   两者要自洽但**不重复**(强队 70% 胜,不代表爆冷就是 30%;若那 30% 多是平局且催化剂不足,爆冷可能更低;
   若弱队动机 + 门将 + 克制打法齐备,爆冷可显著高于弱队纯胜率)。
5. **校准提醒**:冷门不罕见,但别逢强必疑。强弱分明的多数场次落在 **~10–25%**;
   真正势均力敌或催化剂齐备时才更高(30%+)。**联赛中冷门概率较低**(强队持续性强,
   单场方差 < 杯赛)，但仍需保持底线。
6. **爆冷→选项绑定(硬钩子,2026-06-23 教训).** 爆冷概率**不是只供展示**。落选项时必须回流:
   ① totals 给 under 尾部配重(若机制是低分闷杀)+ 置信封顶 ~45;② 半全场给"半场闷"权重 + 置信
   封顶 ~40;③ 爆冷 ≥~28–30% + 具体机制 → 1X2 走双选。**算完爆冷后回到 step 3 按此调整,别让它
   停在备注里。**

### 4. Confidence scoring (0–100,用户定制 ⑤)
给每个预测一个 **0–100 的置信度分值**,由该玩法相关分布的集中度决定(熵 / 到次优选项的差距),
**不再用"高/中/低"标签**。参考映射:75–100=很有把握、55–74=偏向明显、40–54=略微倾向/接近五五、
<40=只作参考。分布越集中→分值越高、该类给的选项越少;分布越平→分值越低、可加到上限。
**同一类里按分值从高到低排序输出(最看好的在前)。**

### 5. Final guardrails (run before writing)
- **Market sanity check.** Lay your 1X2 next to the market's. Big gap with no named mechanism
  → revise toward the market.
- **跨玩法严禁矛盾(硬约束,用户定制 ⑤ — 五项读自同一张比分矩阵,结论必须互相对得上).**
  这不是"软自洽",是**硬约束**:对不上就改到对上为止。逐项核对:
  1. **1X2 方向锁死比分 / 半全场 / 让球的方向。** 押**主胜** → 比分**只能列主胜比分**(禁止平局比分、
     禁止客胜比分);半全场**全场段=胜**;体彩整数让球后的结果同向。押**双选(胜+平)** → 比分只在
     主胜/平局里取,**绝不出现客胜比分**。(用户原话:"说了挪威胜,就不会出现塞内加尔 2-1 挪威"。)
  2. **大小球进球数 = 比分总和。** 比分要**同时**满足两个约束:① 与 1X2 同向,② 总进球落在你给的
     具体进球数上。做法:**先**挑出与 1X2 同向的头部比分,**再**把大小球的进球数取成这些比分的总和。
     例:押主胜 + 比分 2-1(3 球)/2-0(2 球) → 大小球就给"3 球 / 2 球",绝不会冒出一个 1-1=2 球。
  3. 其余:让球大胜 ⇎ 平局倾向;"两队都进球=是" ⇎ 某队零封的头号比分;爆冷概率 ⇎ 1X2。
  Reconcile, don't explain away — 因为同出一张表,一致是算术,不是态度。
- **爆冷对冲自检(2026-06-23 批次教训,硬约束).** 写之前看一眼:**爆冷概率 ≥~25–30% 且我已写出
  具体引爆机制**,但**没有任何一项选项反映它**(totals 没向爆冷尾部配重 / 1X2 没考虑双选 / 半全场
  没给对应权重)?——若是,回去改。**England 0-0 正是"机理写在备注、筹码全压热门"漏做此自检的反例。**
- **高方差市场置信封顶自检.** totals 置信度 >45、半全场 >40 时,反问分布是否真有那么尖——三批数据
  显示这两项系统性过自信;不够尖就压低分值、或在 totals 上多给一个跨线选项。
- **Overconfidence check.** If everything points one way with no downside noted, you've likely
  fallen for the narrative — re-flatten and re-state the opposing case.

---

## 淘汰赛加时 / 点球 (2026-06-28 P2 新增)

> **适用范围：所有淘汰赛（含世界杯 R32 起、杯赛淘汰阶段、两回合制加时）。小组赛/联赛无此节。**
> **核心原则：五项玩法全部只预测 90 分钟常规时间 + 伤停补时的结果。加时和点球是独立的后缀分析。**

### 1. 为什么五项玩法是 90 分钟

中国体彩（竞彩）的所有标准玩法——胜平负、让球、比分、大小球、半全场——**结算以 90 分钟常规时间（含伤停补时）为准**。加时赛和点球大战的进球不计入。

因此：
- **比分** = 90 分钟结束时的比分（不是 120 分钟）
- **胜平负** = 90 分钟结束时的胜/平/负（平局是合法结果，即使后面对手加时赢了）
- **大小球** = 90 分钟内两队总进球
- **让球** = 90 分钟结果套上让球线
- **半全场** = 90 分钟内的半场+全场

**输出时必须标注：**"以下五项预测均为 90 分钟常规时间结果。"

### 2. 加时赛 1X2 分析（独立可选）

如果用户要求分析加时赛，或你想做完整的淘汰赛研判：

**加时赛的结构特征：**
- 加时赛只在 90 分钟打平后才发生
- 加时赛分上下半场各 15 分钟（共 30 分钟），无伤停补时上限
- 加时赛进球计入总进球（但不计入体彩五项）
- 双方体力已大幅消耗（90 分钟高强度比赛后），节奏通常比常规时间慢
- **加时赛不设"平局"终点**——如果加时赛仍然打平，进入点球大战

**加时赛 1X2 的分析框架：**
- **体力储备（最关键因子）：** 哪一方的板凳深度更深？哪一方在 90 分钟内消耗更大？（高位压迫方消耗 > 低位防守方；被围攻方心理消耗 > 围攻方）
- **换人名额使用情况：** 90 分钟内已换了几个？还剩几个？哪一方有"加时专用换人"（速度型前锋、体能型中场）？
- **心理韧性：** 大赛经验、过往加时赛战绩、点球大战经验预期（知道"加时进不了球就点球"会影响冒险程度）
- **战术变化：** 加时赛通常更开放——先丢球方必须全压，先领先方全线退守。加时赛进球的概率**高于常规时间最后 15 分钟**（因为疲劳导致防守失误↑）
- **加时赛 1X2 概率分布特点：** 平局概率比常规时间低很多（加时赛只有 30 分钟，且有"尽头效应"——知道快结束了，双方都可能搏命）→ 平局 ~30%，胜/负各 ~35%

**加时赛分析输出格式（如用户要求）：**
```
【加时赛 1X2 分析（仅当 90 分钟平局时触发）】
预测：XX 胜 / 平 / XX 胜  置信度：NN/100
驱动：体力消耗对比 + 换人储备 + 心理韧性 + 加时赛历史
```

### 3. 点球大战分析框架（独立可选）

点球大战只在加时赛后仍然平局时才发生。点球是**最高方差**的足球事件——从 1X2 方向到单个点球的命中，统计信号极其有限。分析时只关注结构性因子。

**点球大战的核心因子（按权重降序）：**

| 因子 | 权重 | 如何拉取 |
|---|---|---|
| **点球手储备** | ⭐⭐⭐ | Channel B/C：队内常规点球手（近 2 赛季点球命中率）、是否有 5 个以上可靠点球手（加时赛后可能需要第 6-8 人） |
| **门将扑点能力** | ⭐⭐ | Channel D/C：门将职业生涯扑点率（均值 ~20%），样本需 >15 次才有参考价值；单届大赛的小样本忽略 |
| **点球大战经验** | ⭐⭐ | 球队近 5 年是否经历过点球大战？胜率？老将 vs 年轻队的心理差异 |
| **心理/疲劳** | ⭐ | 刚刚踢了 120 分钟的球员 vs 替补上来的球员；谁主动要求踢（自信）vs 被指定踢（被动） |
| **先踢优势** | ⭐ | 先踢方胜率 ~55-60%（统计规律，非因果），但样本小且受时代影响，不应过度依赖 |

**点球分析输出格式（如用户要求）：**
```
【点球大战分析（仅当加时赛后仍平局时触发）】
先踢方：XX 队 / XX 队（取决于硬币抛掷，赛前未知）
点球手储备：XX 队更优？（具体名单 + 命中率）
门将扑点对比：XX 队 GK 生涯扑点率 A% vs B%
经验/心理：XX 队过往点球战绩
综合倾向：XX 队稍优 / 均势 / 无足够信号  置信度：≤35（点球本质是抽签）
```

### 4. 淘汰赛完整预测路径（流程图）

```
90 分钟常规时间 → 五项玩法预测（必出）
    │
    ├─ 某队胜 → 比赛结束
    │
    └─ 平局 → 加时赛（如用户要求或整体研判需要，可追加加时 1X2）
                  │
                  ├─ 某队胜 → 比赛结束
                  │
                  └─ 平局 → 点球大战（如用户要求，追加点球分析）
```

### 5. 与常态预测的交互

- **比分预测必须注明「90 分钟」**，例如 "1–0 阿根廷（90 分钟）" 或 "1–1（90 分钟平局，加时待定）"
- **大小球的 E[goals] 公式**在淘汰赛场景中已默认 ×0.88（见重排 §0.2），因为淘汰赛进球率系统低于小组赛
- **爆冷概率在淘汰赛中上调**：加时/点球的存在使"弱队晋级"的概率高于"弱队 90 分钟赢球"的概率。爆冷概率应额外 +3-5% 来反映"90 分钟逼平 → 加时/点球偷鸡"的路径

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
