### 阶段 06: Spec Synthesis — 需求规格合成与下游门禁交接
# Phase 6: Spec Synthesis - Generating Final Artifacts & Handoff

本阶段的目标是**将用户作答后的所有结构化信息，自动合成为高水平的《需求规格与未知项清单》文档，并输出机器可读的 JSON 供 Confirm-Quiz 门禁消费，完成无缝交接**。

---

## 🎯 规格合成指令 (Synthesis Instructions)

当满足收敛条件时，请自动执行以下两项任务：

### 1. 合成人类可读的 Markdown 规格 (`./docs/spec_<slug>.md`)
- 严格遵循 `templates/spec_example.md` 的版式与结构。
- 将用户的口头回答梳理为标准的技术设计语言（例如，将"Stripe 统一采用 PaymentIntents，前端跳转"整合进数据流描述，而非直接贴聊天记录）。
- 重新核算未答完问题的最新风险评级，将其标注在各象限中。

### 2. 合成机器可读的 JSON 规格 (`./.known-unknown/spec_<slug>.json`)
- 严格遵循 `templates/spec.schema.json` 定义的 JSON 模式。
- 确保所有 `items` 均有正确的 `id`、`quadrant`、`status`（更新为 `answered` 或保持 `open`）、以及 `answer`、`answered_at`。

---

## 🚦 下游 Confirm-Quiz 门禁交接 (Confirm-Quiz Handoff)

在对话的最后，**禁止**给出模棱两可的"你可以开始写代码了"建议。你必须输出一段极具仪式感的门禁指令，作为本 Skill 的闭环：

```markdown
## 🚦 盲点扫描已通过，请进入 Confirm-Quiz 下游门禁

本次需求的所有 Critical (Score >= 8) 盲点已全部澄清收敛。已为您生成：
- 📄 《需求规格与未知项清单》: `./docs/spec_{{ SLUG }}.md`
- 🤖 机器规格接口文件: `./.known-unknown/spec_{{ SLUG }}.json`
- 📊 可视化交互报告: `./.known-unknown/report_{{ SLUG }}.html`

请通过以下指令，一键唤醒搭档技能 `confirm-quiz` 进入实现前的计划（Plan）阶段：

>>> confirm-quiz plan --spec ./.known-unknown/spec_{{ SLUG }}.json

⚠️ 【安全门禁提醒】：在通过 confirm-quiz 的测试（Verify）前，请勿直接合并或交付代码！
```
