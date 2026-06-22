# Reflection Protocol (反思协议 — 结果复盘的固定动作)

**触发(Trigger):** 当用户说"对结果反思 / 复盘 / 这场结算一下 / 看看真实数据对比 / 我预测得怎么样 /
反思并更新"等**指向已发生比赛的复盘意图**时,运行本协议。这是一个**确定性的例行程序**(persistence →
reflection → memory → summary → Why),不要临时发挥、不要只口头说说。

> 一句话:**每次"反思"= 抓真实结果 → 逐项打分 → 写回记忆 → 跨场找系统性偏差 → 追问 Why → 落盘成
> 下次会载入的弱先验。** 落盘是硬要求,不是可选项。

---

## 0. 先确认"真实"数据(不许凭印象)
对每场要复盘的比赛,**联网取**(`references/sources.md` 的 Tier-1 优先:FIFA/官方/ESPN/Opta):
- **最终比分 + 半场比分**(半全场必须要 HT)。
- **进球者与时间、方式**(运动战/定位球/点球/乌龙/越位被吹)。
- **真实首发阵容**,与你赛前"预计阵容"逐一核对(关键人是否首发/缺阵/红黄牌)。
- **底层数据**(可得则取:xG、射门/射正、控球、关键事件如红牌/伤退/VAR)。
绝不用记忆代替查证;查不到就标注缺失,别编。

## 1. 持久化:定位"真正的"技能目录(关键,曾踩坑)
本技能可能存在**同名副本**(如 `World-Cup-Analysis-Skills-Claude` vs `worldcup-analysis-skills`),
且某些副本是**会被重置的临时覆盖层**(写入后下个回合消失)。所以:
- 先 `Get-ChildItem`/glob 找出**含完整 `references/` + `README.md` + `memory/records/_TEMPLATE.md` 的那个目录**
  ——那才是**持久**的真身;只往它写。
- 校验:写完后重新列目录确认文件确实在真身里、且字节数合理。
- 若发现自己在写临时副本,**改写真身目录**并说明。

## 2. 逐场结算(settle each record)— 写进 `memory/records/<file>.md`
对每条匹配的 `pending` 记录(`memory/index.md` 里 kickoff 已过的),按 `memory-protocol.md`:
- 填 **`## Actual`**:最终比分、HT、进球者/时间/方式、**赛前预计阵容 vs 真实阵容**、两队状态对照。
- 填 **`## Reflection`**:
  - **逐市场判定**:胜平负 / 让球 / 比分 / 大小球 / 半全场,各 ✅/❌(双选/不败等对冲也要分别记)。
  - **打分**:对每个概率市场算 **Brier**(`Σ(p−actual)²`)与 **log-loss**(`−ln(p_实际)`);至少给 1X2、
    让球、大小球三项的近似分。
  - **DIAGNOSIS(归因到渠道)**:`news-miss(F) / strength-misrating(A·C) / tactical-misread(E) /
    recency-bias(精排) / overcorrection(追逐上一场) / pure-variance`。**诚实区分"模型错"与"运气"**
    ——3-0 的半场说明是碾压(模型错),最罕见的半全场格被命中/错过多半是 variance。
  - **一句可复用教训**。
- 更新 **`## Transfer notes`**;用 `[[other-record-name]]` 连接相关场次。
- 头部 `status: pending → settled`。
- 回 `memory/index.md` 把该行改为 settled,填 `actual / hit? / one-line lesson`。

## 3. 跨场总结(summary)— 写进 `memory/calibration-summary.md`
不要停在单场。**把本批次所有场次按市场汇总**,找**系统性**信号(单场是噪声,跨场才是偏差):
- 命中分布表(逐市场 X/N)。
- **是否存在同方向偏差**?(例:本批次"四场全押小球 + 受让方"=组合级偏差,而非四个独立判断。)

## 4. 追问 Why(根因)— 用 5-whys,落进同一份 summary
不要停在"我错了/运气差"。逐层下钻到**可改的流程缺陷**:
1. 表面错在哪(哪个市场、哪个方向)?
2. 为什么每场都这样(是不是同一套叙事模板复制)?
3. 这套模板从哪来(是不是小样本/近因外推)?
4. 为什么它压过了市场锚(是不是把定性故事当成强于盘口/xG 的证据)?
5. 结构上缺了什么 guard(帽子不够紧?没有组合级偏差自检?)?
> 同时写**反面教训**:哪些直觉其实是对的、别误伤(避免"为纠一个错而矫枉过正"——单场结果反向追逐
> 本身就是一种 overcorrection)。

## 5. 产出 standing corrections(下次会被载入的弱先验)
在 `calibration-summary.md` 末尾给出 **带方向+小幅度+帽子** 的修正清单,例如
"杀掉系统性 under-bias:totals 向市场收敛 [magnitude: small]"。规则:
- **弱先验,带帽**:每条都是小幅度,随记录增多再加权;**别因一两场过拟合**。
- **会在下次预测的精排阶段作为 Channel H 载入**(见下)。
- 每个新批次结算后,**复核旧 corrections 是否被新结果支持**:支持→加权,推翻→下调/删除。

## 6. 闭环:让反思真正影响下一次预测
- 在 `SKILL.md` 第 2 步(记忆-consult)与第 5 步(精排)中,**除了逐场记录,还要读 `calibration-summary.md`**,
  把 standing corrections 作为 **Channel H 弱先验**纳入(遵守双重计数与帽子规则)。
- 这样"反思"不是写完就完,而是**下一场预测的输入**——这才是校准闭环。

---

## 诚信红线
- 只对**赛前真实给出的概率**打分,不用事后诸葛改写原判(no hindsight rewrite)。
- 正确的过程输给方差 ≠ 坏过程;把方差误判成模型错会导致**追逐噪声**(overcorrection)。
- 落盘失败(目录被重置等)要**如实告知用户**,并尽力写到持久真身目录。
