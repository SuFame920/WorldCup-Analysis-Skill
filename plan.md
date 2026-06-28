# Improvement Plan — 2026-06-28 小组赛终审 · 全部六个任务完成

> **状态：P0 + P1 + P2 + P3 = 6/6 ✅ 已于 2026-06-28 全部完成。**

---

## 已完成任务汇总

| # | 任务 | 状态 |
|---|---|---|
| P0-1 | Memory 分层（corrections.md + calibration-archive.md） | ✅ |
| P0-2 | 独立预测→比较市场流程（A-F 先 → G 后 → 8% 阈值） | ✅ |
| P1-a | xG 公式驱动大小球（E[goals] 脊柱 + 8 格场景系数） | ✅ |
| P1-b | 均势局专用行为规则（三选一触发 + 逐市场行为表） | ✅ |
| P2 | 淘汰赛加时/点球分支（五项玩法 90 min + 加时 1X2 + 点球框架） | ✅ |
| P3 | 通用联赛适配（去 WC 专属 + 自适应检测 + 联赛特有因子） | ✅ |

---

## P2：淘汰赛加时/点球分支（已落盘）

**文件：** `references/reasoning-and-synthesis.md` → 新增「淘汰赛加时 / 点球」节（约 65 行）

- 五项玩法**全部只预测 90 分钟常规时间 + 伤停补时**
- 加时赛 1X2 分析框架：体力储备 → 换人 → 心理 → 战术变化 → 加时赛平局远低于常规时间（~30% vs ~45%）
- 点球大战五因子：点球手储备 → 门将扑点率 → 大赛经验 → 疲劳 → 先踢优势
- 淘汰赛完整三阶段路径图（90 min → 加时 → 点球）
- 淘汰赛爆冷概率额外上调 +3–5%

**关联修改：** `corrections.md` §C 更新（完整淘汰政策 + totals ×0.88 确认化）

---

## P3：通用联赛适配（已落盘）

**文件修改（7 处，跨 4 个文件）：**

| 文件 | 修改内容 |
|---|---|
| `SKILL.md` | name: `football-analysis-skill`（不再 `worldcup`-only）、competition-agnostic 声明、记忆步骤去硬编码 standalone-tracker |
| `references/recall-channels.md` | Channel D 扩展为通用比赛情景（联赛表压 / 杯赛淘汰 / 小组出线） |
| `references/reasoning-and-synthesis.md` | 精排→新增「比赛类型自适应」表（4 种类型 × 7 维度）+ 联赛特有因子；E[goals] 公式通用化（去掉"World Cup ×2"）；xG 缺失补救新增俱乐部联赛均值表；Cohesion 折扣按俱乐部 vs 国家队自适应；冷门概率区分联赛 vs 杯赛 |
| `references/output-format.md` | 整体方向分析新增联赛/杯赛信道标注 + 淘汰赛五项 90 min 标注 |
| `memory/corrections.md` | 头部注释更新为比赛类型自适应；场景表 #5 去掉"(MD3 only)"；条款 #7 新增联赛特有规律；条款 #8 从"MD3 亚型"升级为通用比赛阶段判断 |

**核心原则不变：贝叶斯脊柱、证据进概率出、防双重计价、反 overconfidence、论证反面、校准闭环——只是去掉了硬编码的"World Cup / MD3 / 已出线"假设。**
