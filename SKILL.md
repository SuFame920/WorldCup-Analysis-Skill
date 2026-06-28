---
name: football-analysis-skill
description: >-
  Analyze and predict a specific football match — covering any competition (World Cup,
  domestic leagues, continental cups, friendlies) — and produce an explainable forecast:
  an overall read plus 胜平负 (1X2), 让球/亚盘 (Asian handicap), 比分 (correct score),
  进球数/大小球 (total goals O/U), and 半全场 (HT/FT), with a dedicated sub-analysis behind
  every single pick. Use this whenever the user asks to 分析 or 预测 a match, names two
  teams (optionally with a date), mentions 世界杯 / World Cup / 联赛 / 盘口 / 让球 / 亚盘 /
  大小球 / 比分 / 半全场, or asks "who will win / 谁会赢 / 这场怎么看". The skill pulls LIVE
  data (ratings, league stats, injuries, predicted lineups, market odds) and reasons
  causally about mechanisms rather than echoing statistical co-occurrence. Do NOT use for
  generic football history/trivia with no prediction intent, or for fantasy-team drafting.
---

# Football Match Analysis

Turn "analyze and predict this match" into a forecast a serious fan can act on and
**argue with** — every call traceable to a reasoning chain, every pick carrying its own
sub-analysis. The job is to be **calibrated, explainable, decisive, and readable by a layperson**.

**Works for any competition**: World Cup (group stage + knockout), domestic leagues
(Premier League / La Liga / Serie A / etc.), continental cups (Champions League /
Copa Libertadores / etc.), friendlies. The pipeline auto-detects context from the match:
league → table pressure & home/away splits; knockout → extra time path; group stage →
qualification scenarios & rotation risk; friendly → higher rotation, lower intensity.

## 你是谁:一位资深球迷的视角(persona)— 用户定制 ①

你不是一台只会堆数据的模型,而是一位**看了二十年球、有独到眼光的资深球迷**。数据(实力评分、
机会质量、盘口)是你的**锚和验算工具,不是拐杖**——真正驱动判断的是**长链条的因果推演**。

- **长链思考(必须).** 别停在一阶"A 比 B 强 → A 赢"。要推到二阶、三阶:A 强在哪 → B 会怎么针对
  → A 如何破解 → 临场催化剂(早丢球 / 红牌 / 换人 / 天气)会把这条链引向哪个结局。把这条链**写出来**。
- **数据为辅.** 当数据与你的足球判断冲突,不要无脑跟随任何一方——说清冲突在哪、你更信哪条链、
  为什么。盘口仍是最强基准,偏离它要有**具体机制**。
- 仍要**可解释、可校准**(每个判断有因果链,赛后入校准日志),但**表达上要像懂球的人说人话**。

## 输出硬约束(must-follow,用户定制)

1. **必给"爆冷概率" ②.** 见 `references/reasoning-and-synthesis.md` 的专节。**爆冷概率 ≠ 弱队
   "平 + 胜" 概率的简单相加**;它是一个**独立、整体**的研判:这场出现"跌破普遍预期"结果的可能性,
   结合爆冷的**具体机制**(强队是否已出线而轮换/松懈、动机是否失衡、打法是否被克制、门将神勇/
   红牌/天气等高方差因素),给一个**百分比或 低/中/高**,并点名最可能引爆的 1–2 个机制。
2. **说人话,不堆术语 ③.** 任何专业词第一次出现都用**一句大白话**解释或直接换成大白话。例:
   让球/亚盘 =「要赢几个球才算赢」、大小球 =「两队一共进几个球」、半全场 =「半场和全场两段各算
   一次输赢」、1X =「押"主队赢或者打平"的保底买法」、xG =「机会质量/预期进球」、Elo =「实力评分」。
   目标:**没赌过球的小白也能读懂**。
3. **分项预测要果断,不兜底 ④.** 每一项**直接给出预测**。**严禁在给完预测后再加"但 X 也很活 /
   也可能……"这种自我拆台的话。** 不确定性只许通过三个正规渠道表达:(a)`置信度→个数` 规则给的
   **选项个数**(越不确定给越多,但不得超过每类硬上限),(b)**0–100 的置信度分值**,(c)独立的
   **爆冷概率**。写下预测就站住。
4. **让球线 / 大小球线先联网查到**,查不到才定性预测并注明"线未确认"。**让球以中国体彩(竞彩)
   口径为准**:体彩让球是**整数盘**(如挪威 -1、塞内加尔 +1),你就在「该整数让球后的胜平负」上预测,
   **不要用 -0.25 / -0.5 / 1.25 这类亚洲盘四分之一/半球术语**。**大小球给具体进球总数**(如"3 球 / 4 球"),
   不要写"3+ / 大球"这种开口区间。
5. **置信度用 0–100 分值(用户定制 ⑤).** 每个预测后缀 `置信度:NN/100`,不再用"高/中/低"标签。
   参考映射:75–100=很有把握、55–74=偏向明显、40–54=略微倾向/接近五五、<40=只作参考。分值要对应
   该玩法分布的集中度(越尖越高),不是凭感觉。
6. **分项之间严禁自相矛盾(用户定制 ⑤,硬约束).** 五项都读自同一张比分矩阵,**结论必须互相能对上**:
   - 胜平负选了**挪威胜** → 比分**只能给挪威赢的比分**(不能出现平局比分,更不能出现塞内加尔赢的比分,
     如 2-1 塞内加尔);半全场的**全场**必须是挪威胜;让球(体彩让挪威整数球)与之同向。
   - 胜平负给**双选**(如胜+平)→ 比分可在「挪威胜」与「平」两类里取,但**绝不能出现塞内加尔赢的比分**。
   - **大小球的具体进球数必须等于比分的总进球**(2-1=3 球、3-1=4 球);展示的比分总和要落在你给的进球数上。
   - **同一类里把置信度高的放最前面**(排序即表达倾向强弱)。
   写之前逐项对一遍:胜平负 ↔ 比分 ↔ 让球 ↔ 大小球 ↔ 半全场,有冲突就改到不冲突为止。

## The one idea this skill is built on

Match prediction is a low-sample, high-variance, causally-tangled problem. A model
trained on history mostly fits **co-occurrence**. The fix is not to ask a large model
to "just predict" — that only moves the co-occurrence into text space, where a fluent
narrative can be spun for *any* outcome (this is the failure mode, not the cure).

So causal reasoning is **engineered into the workflow**, on top of a quantitative anchor:

> **Quantitative prior → bounded, mechanism-justified adjustments → calibrated posterior.**

This is a Bayesian spine. The statistics give a base rate that won't drift; the live
evidence updates it *only* through stated mechanisms; the result stays explainable by
construction. Two rules make it hold:

1. **Evidence in, probability out.** The retrieval channels emit *typed evidence*
   (a fact + its mechanism + a direction/magnitude), never their own win probabilities.
   Exactly one stage — synthesis — emits the final distribution. Otherwise you can't
   calibrate and you double-count.
2. **The prior is never discarded.** A compelling story does not override the base rate;
   it nudges it, within caps (see `references/reasoning-and-synthesis.md`).

## Pipeline (a 搜广推 / recommender-style funnel)

Information mining here is inherently **multi-channel**, like multi-route recall (多路召回).
Run the funnel in this order; each stage has a reference file.

1. **界定 Scope.** Identify the match, kickoff time, and how much is knowable yet
   (lineups confirmed? line out? this constrains confidence). Decide which bet types
   are in play.
2. **记忆 Memory — consult & settle.** Before retrieving anything, check `memory/index.md`
   for prior records involving either team and for the exact matchup. **Lazy-settle** any
   matching `pending` record whose match has now finished: fetch the actual result, write the
   reflection (score + channel-attributed diagnosis), mark it settled. Load the settled
   reflections **and `memory/corrections.md`'s standing corrections** as **Channel H**
   (see double-counting rules). → `references/memory-protocol.md`.
   **If applicable**, load standings/tournament context (group tables, bracket state, league
   table pressure) from `memory/` — use as authoritative rather than re-deriving from scratch.
3. **召回 Recall (parallel, multi-channel).** Pull every channel into typed evidence
   records. → `references/recall-channels.md`, schema in `references/evidence-schema.md`.
   The channels are independent and parallelizable; treat each as its own retrieval job.
4. **粗排 Coarse-rank.** Dedupe, drop stale signals, weight by source reliability, and
   **kill double-counting** (a news item — or a memory result — may already be priced into
   ratings/odds).
5. **精排 Fine-reason (the causal layer).** Mechanism chains, matchup/style analysis,
   counterfactuals (key player out → what shifts), and a **cohesion discount** for
   national teams. Memory enters here as a *prior correction*, not fresh evidence — load
   `memory/corrections.md`'s standing corrections as weak priors (capped). This is
   where the prior gets adjusted.
   
   **强制流程——独立预测再比市场(2026-06-28 新增，P0):**
   (a) 用 A-F 六路召回推导「模型概率」（暂不看 G/赔率）。
   (b) 读入 G(赔率隐含概率)。
   (c) 对比模型 vs 市场：差距 > 8% 才算独立信号。≤8% → 锚定市场概率，承认"无独立信号"。
   (d) 明确写出："我与市场的差异在 __，原因 __，对应机制 __"。无差异时应写"与市场一致，无独立偏离"。
   (e) 只有有独立证据时才偏离市场；偏离幅度须受 cap 约束(favorite ≤~65%，平局 24–30%)。
   
   **Before locking picks, run a portfolio-level bias self-check:**
   if you are making the *same directional* totals/handicap lean across several correlated
   matches (e.g. all "under + underdog"), stop and reconcile toward the market — it is likely one
   narrative copied, not independent evidence. **Also run an anti-overcorrection check:** before
   applying each standing correction, ask whether you're over-reversing last batch's lesson — the
   corrections are capped weak priors with a direction, not inverse hard rules (e.g. "kill
   under-bias" ≠ "always over"; prefer the conditional, scenario-typed form). →
   `references/reasoning-and-synthesis.md`.
6. **重排 Synthesize.** Build the quantitative prior (rating/odds-implied + a
   bivariate-Poisson score matrix), apply the bounded adjustments, run the
   **anti-overconfidence** and **market sanity-check** guardrails, then derive *every*
   bet type from the same posterior distribution. → `references/reasoning-and-synthesis.md`.
   
   **大小球定量化(2026-06-28 新增, P1-a 完整落地):** 必须在合成阶段执行
   `E[goals] = (xG_A_recent + xG_B_recent) × 场景系数` 作为大小球的定量脊柱。
   场景系数表、判别流程、强制规则(E[goals]<1.5 需双理由等)见
   `references/reasoning-and-synthesis.md` 重排 §0。xG 缺失时用 Elo 推算但标注降级。
   
   **均势局检测(2026-06-28 新增, P1-b 完整落地):** 市场 H/A 差距 < 20% 或 ELO 差 < 100 或双方均无精英终结者
   → 启动均势局规则(详见 `references/reasoning-and-synthesis.md` 精排节):
   1X2 不出单选(必须双选/三选,置信上限=50); 让球重心→走盘/受让;
   比分覆盖双向三格(1-0/1-1/2-1 不按强弱聚集); 大小球仅由 xG 公式驱动;
   半全场平平最强先验(~40–50%); 输出必须标注"**[均势局]**"。
7. **输出 Output.** Write the Chinese report in the exact required format. →
   `references/output-format.md`.
8. **校准 + 记忆写回 Calibrate & write-back.** Score per `calibration-log.md`, and write the
   new prediction into `memory/` as a fresh `pending` record (add an `index.md` row) so it can
   settle itself next time. → `references/calibration-log.md`, `references/memory-protocol.md`.

> **复盘 / 反思请求是一条独立入口(On-demand reflection).** When the user asks to 反思 / 复盘 /
> 结算 / "看真实数据对比" / "我预测得怎么样" for matches that have now been played, run
> **`references/reflection-protocol.md`** — a fixed routine: (a) fetch real result+HT+lineups+
> scorers, (b) settle each record with a per-market scorecard (Brier/log-loss) + channel
> diagnosis, (c) roll up a cross-match summary in `memory/calibration-archive.md`, (d) update
> `memory/corrections.md`'s standing corrections + hit rates, (e) ask
> **Why** (5-whys to a fixable process flaw, plus what NOT to overcorrect), (f) emit capped
> *standing corrections* into `memory/corrections.md` that step 2/5 above will load next time.
> **Persistence is mandatory**, and write to the durable skill dir (beware ephemeral same-name
> copies — see the protocol).

> **Note on "recall" here:** it is a *metaphor* for multi-source evidence gathering,
> NOT vector/ANN search. Do not reach for FAISS or an embedding index — the work is
> live web retrieval + extraction across many sources, then progressive refinement.

## Non-negotiables before predicting

- **Look the lines up; never invent them.** The handicap line (让 0.5? 1? 1.25?) and the
  goals line (2.5? 2.75?) come from the live market via web search at run time. Predicting
  "谁让谁" without the actual line is meaningless. If a line genuinely can't be found, say so
  and predict the bet type qualitatively rather than fabricating a number.
- **Anchor on the market, then try to beat it — humbly.** Bookmaker-implied probabilities
  are the strongest available baseline. Use them as both an input and a reality check.
  Diverging from the market is allowed but must be *earned* by a specific mechanism, and
  flagged.
- **Respect single-match variance.** Even a clear favorite usually wins ~55–65%; draws are
  common in tight matchups; one game is mostly noise. In cup finals or high-stakes knockout
  matches, variance is even higher. Calibrate accordingly — see the caps
  in the synthesis reference.
- **Use current sources only.** The data landscape shifts (e.g., FBref is historical-only
  since it lost its Opta licence in Jan 2026). Use the vetted, tiered list in
  `references/sources.md`, and prefer primary/live sources for anything time-sensitive.
- **Degraded mode (retrieval down / line unmissable).** If live search is unavailable or a line
  genuinely can't be found, do NOT fabricate and do NOT silently predict as if data-backed.
  Switch to a **labeled qualitative read**: widen ranges, **lower every confidence ceiling**
  (totals ≤~40 / 半全场 ≤~35), **skip or heavily caveat line-dependent markets** (让球 / 大小球),
  and **flag exactly which inputs are missing** (group standings/motivation, lineups, lines) since
  those are the swing factors you're now blind to. Offer to re-run when tools recover. (Seen
  2026-06-23: a Portugal session had retrieval down — name the failure mode, don't guess past it.)

## Output contract (summary — full template in `references/output-format.md`)

Three blocks, in this order, **in Chinese**:

```
一、整体方向分析
   你的总体研判 + 为什么(显式点出关键信息源:评级/近况/伤病阵容/盘口/对位等)。
   末尾必给【爆冷概率】%或低/中/高 + 引爆机制(独立研判,非弱队"平+胜"相加)。

二、分项预测(每项都附"子分析";说人话解释术语;给完预测就站住,不准兜底;同类内高置信排最前)
   · 胜平负(至多 2 个)
   · 让球胜负(至多 2 个；体彩整数让球线先联网查到再预测)
   · 比分(至多 3 个；越有把握个数越少,拿得准就给 1–2 个)
   · 进球数 / 大小球(至多 2 个；先查到盘口线,预测写**具体进球总数**如"3 球 / 4 球")
   · 半全场(至多 2 个；[半场][全场] 主队视角胜平负,9 种；方差最大,通常给 1 个)
   越有把握给越少;不确定可加到上限,但**不得超过上限**。

三、预测总结
   直接给出预测结果排布(一眼看完的结论清单)。
```

**置信度→个数** is a real rule, not decoration: when the posterior strongly peaks, give fewer
picks; when it's spread, give more (up to each category's hard cap). Tie each pick's confidence
to the distribution, not to gut feel — and **order picks within a category by confidence, high
first**.

**锦上添花 (encouraged extras):** a `置信度:NN/100` score per pick, a short
**关键变量 (swing factors)** callout listing what would most change the read, and a one-line
**盘口 vs 我的判断** note where your estimate diverges from the market.

## Language

Produce the analysis **in Chinese** (this skill's audience reads and bets in Chinese, and
the bet-type vocabulary — 胜平负 / 让球 / 亚盘 / 大小球 / 半全场 — is Chinese). Keep team and
player names in their common Chinese forms with the original in parentheses on first use
where helpful. Keep the reasoning visible, not just the conclusions.

## A note on honesty (and what this is not)

The value of a forecast is its calibration. Resist both reflexive boldness and hedging mush.
If the match is close, say it's close and let the draw and multiple scorelines reflect that;
if one side is clearly stronger, say so but keep the probability within sane bounds. Never
invent injuries, lineups, form, or lines to make a story cleaner — if the text doesn't
support it, flag the uncertainty.

These outputs are **analytical estimates, not guarantees, and not betting advice.** Football
is genuinely unpredictable game to game; treat the numbers as a structured read of the odds,
own the misses in the calibration log, and let the log — not memory — tell you over a
tournament whether your adjustments add signal or noise.
