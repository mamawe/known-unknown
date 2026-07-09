# 《需求规格与未知项清单》（样例）
# Requirement Specification & Unknowns List

**项目名称**: 跨境电商支付网关重构 (International Payment Gateway Refactor)  
**文件状态**: 草稿 (Draft)  
**扫描时间**: 2026-07-09  
**关联分支**: `feature/payment-refactor-v2`  

---

## 1. 需求与边界 (Intake & Boundaries)

### 原始需求
> "我们需要重构目前的 Stripe 与 PayPal 支付对接模块，支持多币种自动路由、本地支付方式（如 IDEAL、Alipay），并提升系统的整体稳定性与错误重试策略。"

### 明确范围
- **IN SCOPE (在册范围)**:
  - 统一支付路由接口设计，支持配置化币种分配。
  - Stripe 与 PayPal 适配器（Adapters）的重写。
  - Webhook 接收端重构，引入本地分布式消息队列实现重试。
- **OUT OF SCOPE (非在册范围)**:
  - 账单生成系统与发票系统的重构（只调用已有 API）。
  - 前端支付 UI 页面（使用 Stripe Elements 既有方案，不做定制）。

---

## 2. 盲点扫描四象限 (Four-Quadrant Unknowns)

### 🔴 象限 I：未知的未知 (Unknown Unknowns)
*最关键的技术风险，可能阻碍业务上线，需要高优澄清。*

#### [U-001] Stripe 的三维安全校验 (3DS) 与多币种本地路由冲突
* **维度**: D06_security_privacy (安全与隐私保护)
* **论证与证据**: 经检查 `src/services/stripe.js` 现有版本，支付流程均为直扣模式。引入多币种路由和本地支付方式（如 IDEAL）后，在欧洲区域必须强制开启 3D Secure 认证，这会导致原有的同步支付 API 变成异步跳转流程，导致系统原有的支付状态机失效。
* **风险评估**: Impact: **Critical** | Likelihood: **High** | Reversibility: **Hard** | **Score: 18**
* **澄清结果 (用户回答)**: *Stripe 统一采用 PaymentIntents API，前端由 Stripe SDK 托管 3DS 跳转。后端仅负责处理 webhook 状态变更，原支付 API 降级为只创建 Intent。*

#### [U-002] 跨境汇率波动造成的对账金额不一致风险
* **维度**: D01_data_model (数据模型与状态)
* **论证与证据**: 现有系统的 `orders` 表只存储了单一币种 `USD` 字段。如果支持多币种支付，而对账系统（由第三个系统负责）仍按固定汇率处理，会有大量汇损和错账无法自动核销。
* **风险评估**: Impact: **High** | Likelihood: **Medium** | Reversibility: **Hard** | **Score: 9**
* **澄清结果 (用户回答)**: *数据库中新增 `payment_currency` (实际支付币种) 和 `exchange_rate` (交易时刻汇率) 两个字段。以交易时刻的汇率为准落库，对账时使用此固定汇率。*

---

### 🟣 象限 II：未知的已知 (Unknown Knowns)
*隐藏在代码库中但没有写在需求里的隐性契约与既有风格。*

#### [U-003] 支付适配器的接口命名与报错约定
* **维度**: D11_conventions (隐性规范与代码风格)
* **论证与证据**: 检查 `src/adapters/` 发现目前虽无正式文档，但旧版代码严格继承了 `BasePaymentAdapter` 并且在失败时均抛出 `PaymentException` 而非 generic Error，且异常对象内包含 `rawProviderResponse` 字段。
* **风险评估**: Impact: **Medium** | Likelihood: **High** | Reversibility: **Easy** | **Score: 6**
* **澄清结果 (用户回答)**: *新适配器必须继续继承 `BasePaymentAdapter`。所有 Stripe/PayPal API 原始报错必须包裹进 `PaymentException` 以便全局监控和报警。*

---

### 🟡 象限 III：已知的未知 (Known Unknowns)
*用户本身知道不确定并明确列出的问题。*

#### [U-004] 应该支持哪些具体币种的配置化路由？
* **维度**: D02_boundaries (模块边界与接口契约)
* **风险评估**: Impact: **Medium** | Likelihood: **High** | Reversibility: **Easy** | **Score: 6**
* **澄清结果 (用户回答)**: *首期仅支持 USD, EUR, GBP, JPY。配置方式采用 Apollo 配置中心动态下发 JSON。*

---

### 🟢 象限 IV：已知的已知 (Known Knowns)
*明确定义的无争议需求地图。*

* 统一使用 HTTPS 传输支付数据。
* PayPal Webhook 必须通过 IP 验证与签名前置校验。

---

## 3. 下一步计划 (Next Steps)
1. 运行 HTML 报告补全其他 open 状态的未知项。
2. 答案收敛后，将规格同步至团队 Wiki。
3. 执行：`confirm-quiz plan --spec ./.known-unknown/spec_payment_refactor.json` 开始安全实现。
