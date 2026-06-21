---
name: world-cup-analysis
description: >-
  Analyze and predict a specific football match — especially 2026 FIFA World Cup
  fixtures — and produce an explainable forecast: an overall read plus 胜平负 (1X2),
  让球/亚盘 (Asian handicap), 比分 (correct score), 进球数/大小球 (total goals O/U), and
  半全场 (HT/FT), with a dedicated sub-analysis behind every single pick. Use this
  whenever the user asks to 分析 or 预测 a match, names two teams (optionally with a
  date), mentions 世界杯 / World Cup / 盘口 / 让球 / 亚盘 / 大小球 / 比分 / 半全场, or asks
  "who will win / 谁会赢 / 这场怎么看". The skill pulls LIVE data (ratings, league stats,
  injuries, predicted lineups, market odds) and reasons causally about mechanisms
  rather than echoing statistical co-occurrence. Do NOT use for generic football
  history/trivia with no prediction intent, or for fantasy-team drafting.
---

# World Cup Analysis

Turn "analyze and predict this match" into a forecast a serious fan can act on and
**argue with** — every number traceable to evidence, every pick carrying its own
reasoning. The job is not to sound confident; it is to be **calibrated and explainable**.

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
   reflections as **Channel H** (see double-counting rules). → `references/memory-protocol.md`.
3. **召回 Recall (parallel, multi-channel).** Pull every channel into typed evidence
   records. → `references/recall-channels.md`, schema in `references/evidence-schema.md`.
   The channels are independent and parallelizable; treat each as its own retrieval job.
4. **粗排 Coarse-rank.** Dedupe, drop stale signals, weight by source reliability, and
   **kill double-counting** (a news item — or a memory result — may already be priced into
   ratings/odds).
5. **精排 Fine-reason (the causal layer).** Mechanism chains, matchup/style analysis,
   counterfactuals (key player out → what shifts), and a **cohesion discount** for
   national teams. Memory enters here as a *prior correction*, not fresh evidence. This is
   where the prior gets adjusted. → `references/reasoning-and-synthesis.md`.
6. **重排 Synthesize.** Build the quantitative prior (rating/odds-implied + a
   bivariate-Poisson score matrix), apply the bounded adjustments, run the
   **anti-overconfidence** and **market sanity-check** guardrails, then derive *every*
   bet type from the same posterior distribution. → `references/reasoning-and-synthesis.md`.
7. **输出 Output.** Write the Chinese report in the exact required format. →
   `references/output-format.md`.
8. **校准 + 记忆写回 Calibrate & write-back.** Score per `calibration-log.md`, and write the
   new prediction into `memory/` as a fresh `pending` record (add an `index.md` row) so it can
   settle itself next time. → `references/calibration-log.md`, `references/memory-protocol.md`.

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
- **Respect World Cup variance.** Even a clear favorite usually wins ~55–65%; draws are
  common in tight matchups; one game is mostly noise. Calibrate accordingly — see the caps
  in the synthesis reference.
- **Use current sources only.** The data landscape shifts (e.g., FBref is historical-only
  since it lost its Opta licence in Jan 2026). Use the vetted, tiered list in
  `references/sources.md`, and prefer primary/live sources for anything time-sensitive.

## Output contract (summary — full template in `references/output-format.md`)

Three blocks, in this order, **in Chinese**:

```
一、整体方向分析
   你的总体研判 + 为什么(显式点出关键信息源:评级/近况/伤病阵容/盘口/对位等)。

二、分项预测(每项都附"子分析",比整体更细,且必须可解释)
   · 胜平负(1–2 个)
   · 让球胜负(1–2 个；让球线先联网查到再预测)
   · 比分(2–3 个；越有把握个数越少,拿得准就给 2 个)
   · 进球数 / 大小球(2 个；大小球线先联网查到)
   · 半全场(1–2 个；[半场][全场] 主队视角胜平负,9 种；方差最大,谨慎,通常给 1 个)
   置信度越高,每类的个数越少。

三、预测总结
   直接给出预测结果排布(一眼看完的结论清单)。
```

**置信度→个数** is a real rule, not decoration: when the posterior strongly peaks on one
scoreline, give 2 scores; when it's spread, give 3. Tie each pick's confidence to the
distribution, not to gut feel.

**锦上添花 (encouraged extras):** a 置信度/风险 tag per pick (高/中/低), a short
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
