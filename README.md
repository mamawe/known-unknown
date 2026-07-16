# 已知的未知（Known Unknowns-code）

> 动手写代码之前，先把信息差摊开。

一个在动手写代码**之前**先把信息差摊开的技能：把用户与模型之间的认知缺口拆成四个象限，生成一份交互式「未知项扫描报告」，引导你逐一回答关键问题，最终产出《需求规格与未知项清单》，并硬连接至 `confirm-quiz` 门禁。它不写业务代码，只负责**让你先搞清自己不知道什么**。

---

## 一、依据与逻辑

> 本节的方法论源自 Anthropic 工程师 Thariq Shihipar 的文章 [*A Field Guide to Claude Fable 5: Finding your Unknowns*](https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns)（原帖：[@trq212](https://x.com/trq212/status/2073100352921215386)）。下面只提取其用意，未照搬原文。

### 1. 地图不是疆域

核心隐喻：你给模型的提示词 / 技能 / 上下文只是**「地图」**，真正要改动的代码库、现实约束才是**「疆域」**。**两者之间的差距，就是未知项。**

用意：
- 模型一旦撞上未知项，只能按「你大概想要什么」去猜；你委托它做的事越多，它碰到的未知项就越多。
- 工作质量的上限，往往卡在「你能把未知项澄清到什么程度」，而不在模型本身。
- 光靠提前规划不够——未知项常在实现深处才暴露，或反过来提示你「其实该换个思路解」。
- 因此「发现未知项」是贯穿动手前 / 中 / 后的持续过程，不是一次性动作。

![地图与疆域：地图（提示/技能/上下文）与疆域（真实代码库与约束）之间的差距即未知项](https://cdn.prod.website-files.com/68a44d4040f98a4adf2207b6/6a4be4919e159adcdfa3ec1c_94358c3c.png)

*图：地图 / 疆域 / 未知项 的关系框架。图源：Thariq Shihipar《Finding your Unknowns》原文，版权归原作者所有。*

### 2. 核心逻辑：四象限模型

把所有「地图与疆域的差距」归为四类，本技能即围绕这四种差距展开：

| 象限 | 含义 | 用意 |
|------|------|------|
| 🟢 **已知的已知** (Known Knowns) | 你明确写在需求里的内容 | 已经讲清，直接实现 |
| 🟡 **已知的未知** (Known Unknowns) | 你知道自己还没搞清楚的问题 | 引导你回答、澄清后再实现 |
| 🟣 **未知的已知** (Unknown Knowns) | 代码库里的隐性规范（扫代码发现）| 扫出来呈现给你确认 |
| 🔴 **未知的未知** (Unknown Unknowns) | 完全没想过的盲点（blind spot pass 发现）| 主动推演去发现 |

用意：顶尖的 agentic coder 未知项很少，因为他们既吃透代码库、又与模型行为同频；但他们仍**主动假设未知项的存在**。其中 🟡 与 🔴 最危险——你不仅不知道答案，甚至不知道**该问什么问题**。

![四象限：把未知项拆成 Known Knowns / Known Unknowns / Unknown Knowns / Unknown Unknowns](https://cdn.prod.website-files.com/68a44d4040f98a4adf2207b6/6a4be4919e159adcdfa3ec52_cc0ee2de.png)

*图：未知项四象限矩阵。图源：Thariq Shihipar《Finding your Unknowns》原文，版权归原作者所有。*

### 3. 核心动作：Blind Spot Pass

当你要改不熟悉的代码、或做没把握的事（比如迭代表设计），你会有一大堆 **unknown unknowns**——甚至不知道该问什么、好结果长什么样、历史踩过哪些坑。

文章的解法，是让模型在动手前替你做一次 **blind spot pass（盲点扫描）**，把这些没想到的未知项找出来讲给你听。作者特意强调用 *"blind spot pass"* 和 *"unknown unknowns"* 这两个词来触发——这也就是本技能的启动关键词。

### 4. 隐性规范：Unknown Knowns

还有一类差距是 **unknown knowns**：那些你「看到才认得出来」的隐性标准——已有的错误处理方式、命名规范、架构约定。它们存在于代码库却没人写进文档，模型不主动读就会按自己的默认风格写，产出和项目格格不入。

用意：越早在原型阶段把这些隐性规范「说出来」，代价越低；等到实现中途才发现，返工成本很高。本技能通过**扫描代码库**（Repo Scan 阶段）来发现它们，而不是等你在对话里逐条交代。

### 5. 给模型上下文：平衡指令的尺度

为什么不能「甩个需求就让模型开干」？文章的用意是——指令是一门微妙的平衡：

- **太具体**：模型会照做，哪怕其实该转弯（pivot）更合适。
- **太模糊**：模型按行业最佳实践自行假设，往往并不贴合你的真实场景。
- 不把未知项纳入考量，会「两头落空」：既不知道路上有坑，也不知道路是通的，却仍希望模型能自己绕开。
- 最关键的是**交代你的起点**：你处在思考的哪一步、对问题和代码库熟悉多少，把模型当「思考伙伴」而非纯执行器。

原文把「发现未知项」整理成一套贯穿全程的模式：动手前有 blind spot pass / 头脑风暴 / 访谈 / 参考 / 实现计划，动手中有实现笔记，动手后有讲解 / 测验。本技能聚焦其中的 **动手前 blind spot pass 与实现计划**，并把 **动手后测验** 交给搭档技能 `confirm-quiz`。

> 文章的收束之语：**「减少并规划你的未知项，本身就是 agentic coding 的技能」**——而且这是可以通过和模型协作练出来的。

![减少并规划未知项，本身就是 agentic coding 的技能](https://cdn.prod.website-files.com/68a44d4040f98a4adf2207b6/6a4be4919e159adcdfa3ec55_c4646783.png)

*图：agentic coding 的本质是「减少并规划未知项」。图源：Thariq Shihipar《Finding your Unknowns》原文，版权归原作者所有。*

### 6. 它如何工作：六阶段

`known-unknown` 把上面的逻辑固化为 **6 个阶段**，对应「接收需求 → 扫隐性规范 → 推演灾难 → 优先级排序 → 引导作答 → 产出规格」：

| 阶段 | 核心动作 |
|------|----------|
| 01 Intake | 锁定需求范围和边界，防止扫描跑偏 |
| 02 Repo Scan | 沿 12 维度扫描代码库，找到 Unknown Knowns（隐性规范） |
| 03 Blind Spot Pass | 防守性灾难推演，找到 Unknown Unknowns（从没想到的风险） |
| 04 Risk Scoring | 三维评分（Impact × Likelihood × Reversibility），优先级自动排序 |
| 05 Dialogue | 按优先级逐条引导作答，答案实时写入 localStorage |
| 06 Synthesize | 生成 spec.md + spec.json，交接 confirm-quiz |

### 7. 扫描维度（12 类）

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

### 8. 文件结构

```
known-unknown/
├── SKILL.md                 # 技能定义（触发语义 / 工作流 / 衔接说明）
├── prompts/                 # 01~06 各阶段提示词 + blindspot.md
├── taxonomy/                # scan_dimensions.yaml / risk_rubric.yaml
├── templates/               # spec.schema.json / unknowns_report.html
└── docs/METHODOLOGY.md      # 方法论节选与框架说明
```

---

## 二、如何用

### 1. 什么时候用它

- 你要重构某个模块，但说不清哪些地方有隐患。
- 你和 AI 协同写代码，但担心 AI 在没弄清上下文的情况下乱改。
- 你想做一次系统性的技术风险盘点。

### 2. 一句话启动

```
blind spot pass — [描述你想做的事]
```

### 3. 产物一览

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

### 4. 下一步：接入 confirm-quiz 门禁

扫描产出规格后，本技能会自动引导你通过搭档技能 [confirm-quiz](https://github.com/mamawe/confirm-quiz) 的三阶段门禁（Plan → Implement → Verify）。**未通过 Verify 阶段，不允许合并代码。**

### 5. 常见问题 FAQ

**Q: 扫出 30+ 条未知项，我该从哪里开始？**
A: 只看 🔴 Critical（Score ≥ 8）的条目，其他的先折叠。HTML 报告顶部有「只看红色」过滤器，一键聚焦。

**Q: 代码库很大，扫描特别慢怎么办？**
A: 在 Intake 阶段用 `--scope src/payments/` 形式限定扫描路径，Repo Scan 只会分析指定目录。

**Q: 某些维度对我的项目完全不适用？**
A: 在扫描时明确标注「[D04_concurrency] 本次需求为只读接口，无并发风险，跳过」即可，有文字记录胜于空白。

**Q: 和 confirm-quiz 是什么关系？**
A: known-unknown 负责「搞清楚要做什么」，confirm-quiz 负责「验证做对了没有」。两者是串联的，known-unknown 的 spec.json 是 confirm-quiz 的启动参数。

---

## 三、如何安装

**方式 A — 从 GitHub 克隆后打包导入**

```bash
git clone https://github.com/mamawe/Known-Unknowns-code.git
cd Known-Unknowns-code && zip -r Known-Unknowns-code.zip .
```

在技能市场点「上传技能」，导入生成的 `known-unknown.zip`。

**方式 B — 直接放进技能目录（绕过 UI）**

- **Marvis（macOS）**：`~/Library/Application Support/com.tencent.mac.marvis/MarvisData/User/default_user/skills/custom/`
- **WorkBuddy**：`~/.workbuddy/skills/`
- **Claude Code 类**：`.claude/skills/` 或用户级 skills 目录

复制后**重启应用**生效。

---

## 出处 / 参考

- Thariq Shihipar, *A Field Guide to Claude Fable 5: Finding your Unknowns*, Anthropic Blog
  https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns
- 原帖：https://x.com/trq212/status/2073100352921215386
- 搭档技能：[confirm-quiz](https://github.com/mamawe/confirm-quiz)
- 方法论节选详见本仓库 [`docs/METHODOLOGY.md`](./docs/METHODOLOGY.md)

> 本节仅提取 Thariq Shihipar 原文中与「发现未知项（blind spot pass / unknowns）」相关的用意，未照搬全文。三张配图版权均归原作者所有。
