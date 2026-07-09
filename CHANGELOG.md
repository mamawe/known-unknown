# 更新日志 (Changelog)

## v2.0.0 — 2026-07-10

### 新增
- 新增 `taxonomy/scan_dimensions.yaml`：12 类受控扫描维度词表，把 Repo Scan 与 Blind Spot Pass 变成可复现、可 diff 的过程。
- 新增 `taxonomy/risk_rubric.yaml`：Impact × Likelihood × Reversibility 三维风险打分规则与颜色阈值。
- 新增 `templates/spec.schema.json`：机器可读的需求规格 JSON Schema，作为 confirm-quiz 的硬契约输入。
- 新增 `templates/spec_example.md`：人类可读的需求规格样例。
- 新增 `examples/case_payment_refactor/`：支付网关重构完整走查示例。
- 新增 `CHANGELOG.md`：版本迭代记录。

### 重构
- 重写 `SKILL.md`：明确触发语义（正向/负向）、六阶段工作流、I/O 契约、与 confirm-quiz 的硬门禁交接格式。
- 拆分提示词为 6 个阶段文件：
  - `prompts/01_intake.md`
  - `prompts/02_repo_scan.md`
  - `prompts/03_blindspot_pass.md`
  - `prompts/04_risk_scoring.md`
  - `prompts/05_followup_dialogue.md`
  - `prompts/06_spec_synthesis.md`
- 重写 `templates/unknowns_report.html`：升级为离线单文件 SPA，支持 localStorage 持久化、风险徽章、维度/风险/状态过滤器、键盘导航（j/k/e/Enter/Cmd+S）、导出/导入 JSON、打印视图。

### 改进
- 每个 unknown item 必须携带 `evidence.sources` 与 `evidence.reasoning` 证据字段。
- 引入风险评分 `score`，按 ≥8 / ≥5 / ≥3 / <3 自动分色并排序。
- Dialogue 阶段要求一次只问一条 Critical 项，并支持「不确定」探针模式。
- 与 confirm-quiz 的交接从口头提示升级为结构化命令：`confirm-quiz plan --spec ./.known-unknown/spec_<slug>.json`。

### 移除
- 移除旧版单文件提示词 `prompts/blindspot.md`。
- 移除旧版静态四象限模板 `templates/unknowns_template.html`。
- 清理了意外生成的嵌套目录 `prompts/prompts/`、`taxonomy/taxonomy/`、`templates/templates/`。

---

## v1.0.0 — 2026-07-08

### 新增
- 初始版本：基于 Thariq Shihipar《Finding your Unknowns》的四象限盲点扫描技能。
- 提供 `SKILL.md`、`prompts/blindspot.md`、`templates/unknowns_template.html`。
- 支持生成四象限 HTML 报告并引导用户回答关键问题。
