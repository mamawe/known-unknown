# Example: Payment Gateway Refactor
# 完整走查示例：跨境支付网关重构

这是一个使用 `known-unknown` v2 进行完整盲点扫描的真实走查示例。

---

## 场景背景

**需求描述**：重构现有 Stripe + PayPal 双通道支付模块，支持多币种自动路由与本地支付方式（IDEAL、Alipay），提升稳定性与错误重试策略。

**代码库规模**：中型 Node.js 后端，12 个相关文件，3 个外部 API 依赖。

---

## 走查步骤

### Phase 01 — Intake（需求接收与边界锁定）

**用户原始请求**：
> "重构支付模块，接入多币种路由和本地支付方式，修一下现在的重试机制。"

**扫描产出**：
- **核心目标**：统一支付路由接口、Stripe/PayPal 适配器重写、Webhook 重试重构
- **IN SCOPE**：`src/services/stripe.js`, `src/services/paypal.js`, `src/adapters/`, `src/webhooks/`
- **OUT OF SCOPE**：账单系统、发票系统、前端支付 UI

---

### Phase 02 — Repo Scan（Unknown Knowns 发现）

扫描结果（节选）：

| ID | 发现内容 | 维度 | Score |
|----|---------|------|-------|
| U-003 | 所有适配器均继承 `BasePaymentAdapter`，失败时抛 `PaymentException` | D11_conventions | 6 |
| U-007 | `orders` 表使用软删除（`is_deleted` 字段），不做硬删除 | D01_data_model | 4 |

---

### Phase 03 — Blind Spot Pass（Unknown Unknowns 发现）

推演产出（节选）：

| ID | 风险描述 | 维度 | Score |
|----|---------|------|-------|
| U-001 | IDEAL 支付强制 3D Secure → 原同步支付 API 状态机失效 | D06_security_privacy | 18 |
| U-002 | 多币种支付时汇率未锁，对账系统按固定汇率核算产生系统性误差 | D01_data_model | 9 |

---

### Phase 04 — Risk Scoring（优先级排序）

```
🔴 Critical (Score >= 8):
  U-001: 3DS 与多币种路由冲突       Score: 18
  U-002: 汇率对账不一致风险          Score: 9

🟠 High (Score >= 5):
  U-003: 适配器接口命名约定          Score: 6
  U-004: 币种路由配置方式未确认      Score: 6

🟡 Medium:
  U-007: 软删除联动处理              Score: 4
```

---

### Phase 05 — Dialogue（用户作答记录）

**Q [U-001]**：引入 IDEAL/Alipay 后，Stripe 3DS 验证会把现有同步支付状态机变成异步流程。如果我们按现有模式直接对接，用户支付后状态可能永远卡在 `pending`。
> **用户答**：Stripe 统一采用 PaymentIntents API，前端 SDK 托管 3DS 跳转，后端只监听 webhook 状态变更。

**Q [U-002]**：多币种支付时，`orders` 表只有 USD 字段，对账系统用固定汇率核算，会产生不可核销的汇损。
> **用户答**：新增 `payment_currency` 和 `exchange_rate` 字段，以交易时刻汇率为准落库。

---

### Phase 06 — Spec Synthesis（规格合成）

产物文件：
- `./docs/spec_payment_refactor.md` — 人类可读规格
- `./.known-unknown/spec_payment_refactor.json` — 机器规格
- `./.known-unknown/report_payment_refactor.html` — 交互式报告

交接指令：
```
>>> confirm-quiz plan --spec ./.known-unknown/spec_payment_refactor.json
```

---

## 关键收获

1. **最大收益**：U-001（3DS 冲突）如果没有 Blind Spot Pass，几乎必然在上线后才发现，修复代价极高。
2. **U-002**（汇率对账）通过一次对话完全澄清，节省了大量潜在的财务核查工作。
3. 整个扫描从需求到规格交接，耗时约 **25 分钟**。

---

*本示例已脱敏，仅用于说明完整走查流程。*
