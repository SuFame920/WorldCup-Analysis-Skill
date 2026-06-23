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
  to none.
- **Argue the other side.** Before locking a lean, state the best case for the opposite result.
  If it's strong, the distribution should be flatter (more draw / more alternative scorelines).
  This is the explicit antidote to narrative bias. **这一步的产物在重排阶段汇成「爆冷概率」(见 3.5),
  而不是写成输出端的犹豫措辞。**
- **反矫枉过正自检(anti-overcorrection,2026-06-22 批次教训).** 载入 `calibration-summary.md`
  的 standing corrections(Channel H)作为弱先验时,每应用一条先问:**"我是不是在把上一批次的教训矫枉
  过正?"** 修正是**带方向+小幅度+帽子的弱先验,不是反向铁律**:上次押小错了**不等于**这次无条件押大
  (本批 Arg-Aut 正因把"杀 under-bias"用成"一律押大"而错)。优先用**条件式修正**(如 totals 按场景
  分型)而非无条件标量;一条修正与本场具体机制冲突时,信本场机制,别硬套历史修正。

---

## 重排 — Synthesize (one posterior, then read every bet off it)

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

- **胜平负 (1X2):** sum the matrix into Home / Draw / Away. Report **≤2** picks; if the top
  outcome doesn't clearly dominate, the honest answer may be a "双选/draw-inclusive" lean. **这个
  方向一旦定下,就锁死了比分、半全场、让球的方向**(见 step 5 的不矛盾硬约束)。
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
5. **校准提醒**:世界杯冷门不罕见,但别逢强必疑。强弱分明的多数场次落在 **~10–25%**;
   真正势均力敌或催化剂齐备时才更高(30%+)。

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
- **Overconfidence check.** If everything points one way with no downside noted, you've likely
  fallen for the narrative — re-flatten and re-state the opposing case.

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
