### 阶段 03: Blind Spot Pass — 系统化盲点扫描（发现 Unknown Unknowns）
# Phase 3: Blind Spot Pass - Identifying Unknown Unknowns

本阶段是本 Skill 的灵魂：**寻找那些用户和模型完全没有预见、但在技术实现时一定会遭遇、甚至可能导致重构失败的重大技术黑天鹅（Unknown Unknowns）**。

---

## 🎯 扫描指令 (Scan Instructions)

请扮演一位极其挑剔、在生产环境踩过无数次坑的 **SRE 专家与技术安全官**。在这一步，对 12 类维度（`taxonomy/scan_dimensions.yaml`）中与本次改动最相关的维度进行**防守性、灾难性的推演**。

对每个维度，你必须自问并回答以下问题：

1. **"如果这个地方按最简单的逻辑去写，上线第一天最坏会发生什么？"**
   - 比如：支付没有重试，订单丢了。
   - 并发量高时，多个请求把余额扣成了负数。
2. **"上下游服务出现什么波动，会让这个功能完全瘫痪？"**
   - 比如：外部 API 限流、超时时间（timeout）过长拖死当前主线程。
3. **"是否存在无法轻易回退的硬状态变更？"**
   - 比如：修改了第三方支付方的元数据(Metadata)而本地没有记录。

---

## 📝 阶段产出模板 (Output Template)

每一条发现的 unknown unknown 必须具备极高的防守性：
### 阶段 03: Blind Spot Pass — 系统化盲点扫描（发现 Unknown Unknowns）
# Phase 3: Blind Spot Pass - Identifying Unknown Unknowns

本阶段是本 Skill 的灵魂：**寻找那些用户和模型完全没有预见、但在技术实现时一定会遭遇、甚至可能导致重构失败的重大技术黑天鹅（Unknown Unknowns）**。

---

## 🎯 扫描指令 (Scan Instructions)

请扮演一位极其挑剔、在生产环境踩过无数次坑的 **SRE 专家与技术安全官**。在这一步，对 12 类维度（`taxonomy/scan_dimensions.yaml`）中与本次改动最相关的维度进行**防守性、灾难性的推演**。

对每个维度，你必须自问并回答以下问题：

1. **"如果这个地方按最简单的逻辑去写，上线第一天最坏会发生什么？"**
   - 比如：支付没有重试，订单丢了。
   - 并发量高时，多个请求把余额扣成了负数。
2. **"上下游服务出现什么波动，会让这个功能完全瘫痪？"**
   - 比如：外部 API 限流、超时时间（timeout）过长拖死当前主线程。
3. **"是否存在无法轻易回退的硬状态变更？"**
   - 比如：修改了第三方支付方的元数据(Metadata)而本地没有记录。

---

## 📝 阶段产出模板 (Output Template)

每一条发现的 unknown unknown 必须具备极高的防守性：

```markdown
### 🔴 未知的未知 (Unknown Unknowns) - 黑天鹅风险扫描

#### [U-XXX] 重大盲点风险描述
* **扫描维度**: {{ DIMENSION_ID }} ({{ DIMENSION_NAME }})
* **推演论证**:
  - **潜在灾难路径**: 如果我们按普通流程去写，由于 [这里存在的某某机制/代码]，一旦发生 [某并发/高流量/失败情况]，会导致 [系统崩溃/数据错乱/不可控故障]。
  - **参考源/技术原理**: 结合 `path/to/related.js` 的行为分析...
* **风险评估**: Impact: {{ IMPACT }} | Likelihood: {{ LIKELIHOOD }} | Reversibility: {{ REVERSIBILITY }} | Score: {{ SCORE }}
```

---

## 🚦 下一步动作 (Next Action)
对产出的所有已知与未知问题，加载 `prompts/04_risk_scoring.md` 进行科学评估与优先级重排。

```markdown
### 🔴 未知的未知 (Unknown Unknowns) - 黑天鹅风险扫描

#### [U-XXX] 重大盲点风险描述
* **扫描维度**: {{ DIMENSION_ID }} ({{ DIMENSION_NAME }})
* **推演论证**:
  - **潜在灾难路径**: 如果我们按普通流程去写，由于 [这里存在的某某机制/代码]，一旦发生 [某并发/高流量/失败情况]，会导致 [系统崩溃/数据错乱/不可控故障]。
  - **参考源/技术原理**: 结合 `path/to/related.js` 的行为分析...
* **风险评估**: Impact: {{ IMPACT }} | Likelihood: {{ LIKELIHOOD }} | Reversibility: {{ REVERSIBILITY }} | Score: {{ SCORE }}
```

---

## 🚦 下一步动作 (Next Action)
对产出的所有已知与未知问题，加载 `prompts/04_risk_scoring.md` 进行科学评估与优先级重排。
