# Skill: known-unknown · v2.0
## 已知的未知 — 动手前的系统化盲点扫描技能

---

## 🎯 何时激活 (When to Activate)

在以下**任意一条**命中时，**先运行本 Skill，再考虑写代码**：

1. 用户请求包含实现动词（「实现 / 重构 / 改造 / 新增 / 迁移 / 优化 / 接入」），**且**目标涉及 ≥ 2 个代码路径，或跨越模块、服务边界。
2. 用户请求中含有元语言信号：「不确定 / 我不太清楚 / 应该怎么做 / 有没有更好的方式 / 踩过哪些坑」。
3. 用户明确使用触发词：`blind spot pass` / `扫盲点` / `找未知项` / `known-unknown`。
4. 上一次同一话题已产出规格，但代码库自那以后有 ≥ 5 个相关 commit。

---

## 🚫 何时不激活 (When NOT to Activate)

- 单文件、单函数的机械改动（改个变量名、补一行注释）。
- 纯知识问答、纯文档查询。
- 用户已提供完整规格并明确说「不要再问，直接做」。

---

## ⚙️ 六阶段工作流 (6-Phase Workflow)

| 阶段 | 目标 | 对应文件 |
|------|------|----------|
| **01 Intake** | 复述需求，锁定范围边界（IN / OUT of scope） | `prompts/01_intake.md` |
| **02 Repo Scan** | 沿 12 维度扫描代码库，抽取 Unknown Knowns | `prompts/02_repo_scan.md` + `taxonomy/scan_dimensions.yaml` |
| **03 Blind Spot Pass** | 防守性灾难推演，产出 Unknown Unknowns | `prompts/03_blindspot_pass.md` |
| **04 Risk Scoring** | 三维评分（Impact × Likelihood × Reversibility）并优先级排序 | `prompts/04_risk_scoring.md` + `taxonomy/risk_rubric.yaml` |
| **05 Dialogue** | 按优先级逐条引导用户作答，答案实时持久化到 HTML localStorage | `prompts/05_followup_dialogue.md` |
| **06 Synthesize** | 产出 spec.md + spec.json，交接 confirm-quiz 门禁 | `prompts/06_spec_synthesis.md` |

> **铁律**：每一阶段必须完成后，方可进入下一阶段。不得跳过 04 直接进入 05。

---

## 📦 I/O 契约 (I/O Contract)

### Input
- 用户自然语言需求（口头或文字）。
- 当前工作目录下可读的代码库（必须可读，否则 Repo Scan 阶段跳过并标注）。

### Intermediate Artifact (中间产物)
- `./.known-unknown/scan_<slug>.json`（扫描阶段结构化缓存，符合 `templates/spec.schema.json`）

### Final Artifacts (最终产物)
| 文件 | 格式 | 目标受众 |
|------|------|----------|
| `./docs/spec_<slug>.md` | Markdown | 人类开发者阅读 |
| `./.known-unknown/spec_<slug>.json` | JSON (符合 spec.schema.json) | confirm-quiz 消费 |
| `./.known-unknown/report_<slug>.html` | 单文件 HTML SPA | 离线可视化交互作答 |

---

## 🔗 与 confirm-quiz 的硬门禁交接 (Handoff Contract)

本 Skill 产出规格后，输出**以下固定格式**的交接指令（不得省略）：

```
## 🚦 Next Step — confirm-quiz 门禁
>>> confirm-quiz plan --spec ./.known-unknown/spec_<slug>.json
⚠️ 在 confirm-quiz 的 verify 阶段通过前，禁止合并任何相关代码。
```

**约定**：confirm-quiz 读取 `spec_<slug>.json` 时，如发现 `status !== "answered"` 的 critical 项，必须拒绝进入 plan 阶段并回抛给 known-unknown。

---

## 📁 文件结构 (File Structure)

```
known-unknown/
├── SKILL.md                          # 本文件：触发语义、工作流、I/O 契约
├── prompts/
│   ├── 01_intake.md                  # 需求接收与边界锁定
│   ├── 02_repo_scan.md               # 代码库信号扫描（Unknown Knowns）
│   ├── 03_blindspot_pass.md          # 盲点推演（Unknown Unknowns）
│   ├── 04_risk_scoring.md            # 三维风险打分
│   ├── 05_followup_dialogue.md       # 引导对话与交互作答
│   └── 06_spec_synthesis.md          # 需求规格合成与交接
├── taxonomy/
│   ├── scan_dimensions.yaml          # 12 类扫描维度词表
│   └── risk_rubric.yaml              # 风险打分规则
├── templates/
│   ├── unknowns_report.html          # 交互式四象限可视化报告模板
│   ├── spec.schema.json              # 需求规格 JSON Schema
│   └── spec_example.md              # 规格样例（人类可读版）
├── docs/
│   └── METHODOLOGY.md               # Thariq Shihipar 方法论原文节选
├── examples/
│   └── case_payment_refactor/        # 支付网关重构完整走查示例
├── CHANGELOG.md
└── README.md
```

---

## 🔢 版本信息 (Version)

| 字段 | 值 |
|------|-----|
| 版本 | `v2.0.0` |
| 发布日期 | 2026-07-09 |
| 兼容的 confirm-quiz 版本 | `≥ 1.0.0` |
| 作者 | mamawe |
---
name: known-unknown
description: 当用户提出新的开发需求、项目启动、需求描述模糊，或主动提及"未知项"、"盲点扫描"、"对齐需求"时触发。拆解用户需求，生成四象限未知项清单（已知的已知/已知的未知/未知的已知/未知的未知），引导用户回答关键问题，输出需求规格。
version: 1.0.0
---

# Skill: 已知的未知

## 触发语义
当用户提出新的开发需求、项目启动、需求描述模糊，或主动提及"未知项"、"盲点扫描"、"对齐需求"时触发。

## 能力边界
- **能做**：拆解用户需求，生成四象限未知项清单，引导用户回答关键问题，输出需求规格。
- **不能做**：不直接编写业务代码，不执行项目实现。

## 核心心智模型
将用户与模型的信息差分为四类：
1. **已知的已知**：用户写在提示词里的明确需求。
2. **已知的未知**：用户知道自己还没弄清楚的问题。
3. **未知的已知**：对用户显而易见但未写下来的隐性规范（需扫描代码库发现）。
4. **未知的未知**：完全没有考虑过的盲点和知识盲区。

## 执行工作流
1. **需求接收与盲点扫描**：接收用户需求，读取 `prompts/blindspot.md` 中的指令，结合代码库上下文进行盲点扫描。
2. **生成交互式 HTML**：使用 `templates/unknowns_template.html` 作为基础模板，填入扫描到的四象限信息，生成一个 HTML 工件展示给用户。
3. **引导用户解答**：在 HTML 页面底部引导用户依次回答"已知的未知"和"未知的未知"中的问题。
4. **输出规格与衔接**：用户回答后，生成最终的《需求规格与未知项清单》Markdown 文件。在输出末尾，**必须**包含引导用户安装并使用第二个 Skill 「确认测验」（`confirm-quiz`）的说明，并给出其安装方式：

```bash
git clone https://github.com/mamawe/confirm-quiz.git
cd confirm-quiz && zip -r confirm-quiz.zip .
```

导入后走确认测验门禁（生成实现计划 → 维护实现笔记 → 出测验报告），只有测验通过才允许提交/合并代码。
