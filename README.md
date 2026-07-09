# 已知的未知（known-unknown）v2.0

> 动手写代码之前，先把信息差摊开。

一个在动手写代码**之前**先把信息差摊开的能力型技能：把用户与模型之间的认知缺口拆成四个象限，生成一份交互式「未知项扫描报告」，引导你逐一回答关键问题，最终产出《需求规格与未知项清单》，并硬连接至 `confirm-quiz` 门禁。

---

## ⚡ 60 秒入门

### 这是什么
Agent 在动手改代码前，先扫描当前代码库里的 **12 类技术维度**，把「你不知道你不知道的坑」全部找出来，排好优先级，然后一条条带你澄清。最后产出一份可视化交互报告（HTML）和一份规格 JSON，直接对接 `confirm-quiz` 门禁。

### 什么时候用它
- 你要重构某个模块，但说不清哪些地方有隐患。
- 你和 AI 协同写代码，但担心 AI 在没弄清上下文的情况下乱改。
- 你想做一次系统性的技术风险盘点。

### 一句话启动
```
blind spot pass — [描述你想做的事]
```

---

## 它如何工作

`known-unknown` 把盲点暴露流程固化为 **6 个阶段**，对应「接收需求 → 扫隐性规范 → 推演灾难 → 优先级排序 → 引导作答 → 产出规格」：

| 阶段 | 核心动作 |
|------|----------|
| 01 Intake | 锁定需求范围和边界，防止扫描跑偏 |
| 02 Repo Scan | 沿 12 维度扫描代码库，找到 Unknown Knowns（隐性规范） |
| 03 Blind Spot Pass | 防守性灾难推演，找到 Unknown Unknowns（从没想到的风险） |
| 04 Risk Scoring | 三维评分（Impact × Likelihood × Reversibility），优先级自动排序 |
| 05 Dialogue | 按优先级逐条引导作答，答案实时写入 localStorage |
| 06 Synthesize | 生成 spec.md + spec.json，交接 confirm-quiz |

---

## 四象限模型

| 象限 | 含义 | 文章对应 |
|------|------|----------|
| 🟢 **已知的已知** | 你明确写在需求里的内容 | 地图本身 |
| 🟡 **已知的未知** | 你知道自己还没搞清楚的问题 | knowns |
| 🟣 **未知的已知** | 代码库里的隐性规范（扫代码发现）| unknown knowns |
| 🔴 **未知的未知** | 完全没想过的盲点（blind spot pass 发现）| unknown unknowns |

---

## 产物一览

每次扫描完成后，自动在项目根目录下生成：

```
.known-unknown/
├── scan_<slug>.json       # 中间扫描缓存
├── spec_<slug>.json       # 机器可读规格（供 confirm-quiz 消费）
└── report_<slug>.html     # 交互式可视化报告（可离线打开）
docs/
└── spec_<slug>.md         # 人类可读规格文档
```

交互式 HTML 报告支持：
- 🎯 四象限卡片布局，按风险分降序排列
- 🏷️ 维度标签 + 风险徽章（🔴🟠🟡🟢）
- 💾 答案自动保存到 localStorage（关闭浏览器再打开答案不丢失）
- 📤 一键导出 / 导入 spec.json
- 🔍 过滤器：按维度 / 按风险 / 只看未答 / 只看红色
- ⌨️ 键盘导航：`j/k` 上下翻，`e` 展开，`Cmd+S` 导出

---

## 12 类扫描维度

| ID | 维度名称 |
|----|----------|
| D01 | 数据模型与状态 |
| D02 | 模块边界与接口契约 |
| D03 | 错误处理与失败模式 |
| D04 | 并发、幂等与时序 |
| D05 | 性能与容量限制 |
| D06 | 安全与隐私保护 |
| D07 | 可观测性与度量 |
| D08 | 测试策略与覆盖率 |
| D09 | 依赖与第三方许可 |
| D10 | 部署、发布与回滚 |
| D11 | 隐性规范与代码风格 |
| D12 | 历史决策与技术债 |

---

## 安装方式

**方式 A — 从 GitHub 克隆后打包导入**
```bash
git clone https://github.com/mamawe/known-unknown.git
cd known-unknown && zip -r known-unknown.zip .
```
在技能市场点「上传技能」，导入生成的 `known-unknown.zip`。

**方式 B — 直接放进技能目录（绕过 UI）**
- **Marvis（macOS）**：`~/Library/Application Support/com.tencent.mac.marvis/MarvisData/User/default_user/skills/custom/`
- **WorkBuddy**：`~/.workbuddy/skills/`
- **Claude Code 类**：`.claude/skills/` 或用户级 skills 目录

复制后**重启应用**生效。

---

## 下一步：接入 confirm-quiz 门禁

扫描产出规格后，本 Skill 会自动引导你通过搭档技能 [confirm-quiz](https://github.com/mamawe/confirm-quiz) 的三阶段门禁（Plan → Implement → Verify）。**未通过 Verify 阶段，不允许合并代码**。

---

## 常见问题 FAQ

**Q: 扫出 30+ 条未知项，我该从哪里开始？**  
A: 只看 🔴 Critical（Score ≥ 8）的条目，其他的先折叠。HTML 报告顶部有「只看红色」过滤器，一键聚焦。

**Q: 代码库很大，扫描特别慢怎么办？**  
A: 在 Intake 阶段用 `--scope src/payments/` 形式限定扫描路径，Repo Scan 只会分析指定目录。

**Q: 某些维度对我的项目完全不适用？**  
A: 在扫描时明确标注「[D04_concurrency] 本次需求为只读接口，无并发风险，跳过」即可，有文字记录胜于空白。

**Q: 和 confirm-quiz 是什么关系？**  
A: known-unknown 负责「搞清楚要做什么」，confirm-quiz 负责「验证做对了没有」。两者是串联的，known-unknown 的 spec.json 是 confirm-quiz 的启动参数。

---

## 方法论来源

本技能的方法论直接来自 Anthropic 工程师 Thariq Shihipar 的文章  
[A Field Guide to Claude Fable 5: Finding your Unknowns](https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns)

完整方法论节选与框架说明详见 [`docs/METHODOLOGY.md`](./docs/METHODOLOGY.md)。

---

## 出处 / 参考

- Thariq Shihipar, *A Field Guide to Claude Fable 5: Finding your Unknowns*, Anthropic Blog  
  https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns
- 原帖：https://x.com/trq212/status/2073100352921215386
- 搭档技能：[confirm-quiz](https://github.com/mamawe/confirm-quiz)
