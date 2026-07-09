### 阶段 04: Risk Scoring — 三维风险打分与优先级排序
# Phase 4: Risk Scoring - Prioritizing Unknowns

本阶段的目标是**使用科学、量化的评估指标，为扫描出的所有未知项进行打分和优先级重排**，确保最危险、最底层的问题优先暴露并被回答，避免用户被琐碎问题淹没。

---

## 🎯 评分公式与指令 (Scoring Rubric & Instructions)

请结合 `taxonomy/risk_rubric.yaml` 中的规则，对每一个扫描出的 Item 给出以下 3 个维度的评分并计算最终得分：

$$	ext{Score} = 	ext{Impact} 	imes 	ext{Likelihood} 	imes 	ext{Reversibility}$$

### 1. 影响级别 (Impact Weight)
- **Critical (4)**: 系统瘫痪、敏感数据泄露、不可恢复的故障、全量用户报错。
- **High (3)**: 核心业务功能受阻、大范围功能降级、高风险数据丢失。
- **Medium (2)**: 局部功能异常、用户体验明显受损。
- **Low (1)**: 边缘 UI 缺陷、不影响核心流程的瑕疵。

### 2. 爆发概率 (Likelihood Weight)
- **High (3)**: 极易爆发（如高并发场景、高频调用链路、或者代码中已发现不兼容信号）。
- **Medium (2)**: 可能爆发（中等调用频次，需要特定环境或前置条件触发）。
- **Low (1)**: 极低概率。

### 3. 可逆程度 (Reversibility Multiplier)
- **Irreversible (2.0)**: **不可逆**。上线后修改需要推倒重来、或者会对第三方数据造成污染、或者涉及法律合规风险无法修改。
- **Hard (1.5)**: **困难**。修改需要进行复杂的数据库 Schema 迁移、需要协调多方 API 或重新发布大版本。
- **Easy (1.0)**: **容易**。可通过热修复、功能开关配置、或简单的代码修补立即解决。

---

## 🚦 优先级排序与门禁### 阶段 04: Risk Scoring — 三维风险打分与优先级排序
# Phase 4: Risk Scoring - Prioritizing Unknowns

本阶段的目标是**使用科学、量化的评估指标，为扫描出的所有未知项进行打分和优先级重排**，确保最危险、最底层的问题优先暴露并被回答，避免用户被琐碎问题淹没。

---

## 🎯 评分公式与指令 (Scoring Rubric & Instructions)

请结合 `taxonomy/risk_rubric.yaml` 中的规则，对每一个扫描出的 Item 给出以下 3 个维度的评分并计算最终得分：

$$	ext{Score} = 	ext{Impact} 	imes 	ext{Likelihood} 	imes 	ext{Reversibility}$$

### 1. 影响级别 (Impact Weight)
- **Critical (4)**: 系统瘫痪、敏感数据泄露、不可恢复的故障、全量用户报错。
- **High (3)**: 核心业务功能受阻、大范围功能降级、高风险数据丢失。
- **Medium (2)**: 局部功能异常、用户体验明显受损。
- **Low (1)**: 边缘 UI 缺陷、不影响核心流程的瑕疵。

### 2. 爆发概率 (Likelihood Weight)
- **High (3)**: 极易爆发（如高并发场景、高频调用链路、或者代码中已发现不兼容信号）。
- **Medium (2)**: 可能爆发（中等调用频次，需要特定环境或前置条件触发）。
- **Low (1)**: 极低概率。

### 3. 可逆程度 (Reversibility Multiplier)
- **Irreversible (2.0)**: **不可逆**。上线后修改需要推倒重来、或者会对第三方数据造成污染、或者涉及法律合规风险无法修改。
- **Hard (1.5)**: **困难**。修改需要进行复杂的数据库 Schema 迁移、需要协调多方 API 或重新发布大版本。
- **Easy (1.0)**: **容易**。可通过热修复、功能开关配置、或简单的代码修补立即解决。

---

## 🚦 优先级排序与门禁

1. **🔴 Score >= 8 (Critical / High Risk)**:
   - **硬门禁级**。必须在进入下一阶段（Confirm-Quiz / Plan）前由用户完全解答或明确指派负责人。
2. **🟠 Score >= 5 (Medium-High Risk)**:
   - 强烈建议在 Plan 阶段开始前明确。
3. **🟡 Score >= 3 (Medium Risk)**:
   - 允许在实现阶段逐步补充。
4. **🟢 Score < 3 (Low Risk)**:
   - 可直接记录，在后续迭代中自然沉淀。

---

## 📝 阶段产出模板 (Output Template)

在列表中根据计算出的 Score 降序重排，并输出至 HTML 报告的数据区。


1. **🔴 Score >= 8 (Critical / High Risk)**:
   - **硬门禁级**。必须在进入下一阶段（Confirm-Quiz / Plan）前由用户完全解答或明确指派负责人。
2. **🟠 Score >= 5 (Medium-High Risk)**:
   - 强烈建议在 Plan 阶段开始前明确。
3. **🟡 Score >= 3 (Medium Risk)**:
   - 允许在实现阶段逐步补充。
4. **🟢 Score < 3 (Low Risk)**:
   - 可直接记录，在后续迭代中自然沉淀。

---

## 📝 阶段产出模板 (Output Template)

在列表中根据计算出的 Score 降序重排，并输出至 HTML 报告的数据区。
