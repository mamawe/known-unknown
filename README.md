# 已知的未知（known-unknown）

一个在动手写代码**之前**先把信息差摊开的能力型技能：把用户与模型之间的认知缺口拆成四个象限，生成一份交互式「未知项扫描报告」，引导你回答关键问题，最终产出《需求规格与未知项清单》。它不写业务代码，只负责**让你先搞清自己不知道什么**。

> 英文名 `known-unknown` 取自文章 *Finding your Unknowns* 的核心概念，也是这个技能要解决的那个问题本身。

---

## 设计来源：Thariq Shihipar《Finding your Unknowns》

这个技能的方法论直接来自 Anthropic 工程师 **Thariq Shihipar** 的文章
[*A Field Guide to Claude Fable 5: Finding your Unknowns*](https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns)
（原帖：[@trq212](https://x.com/trq212/status/2073100352921215386)）。

下面只节选了与「发现未知项」这一步骤**直接相关**的内容。

### 1. 地图不是疆域（这个技能为什么存在）

> *"When working with Claude Code, I'm often reminded of the difference between the map and the territory. The map, a representation of the work to be done, is my prompts and skills and context, it's what I give Claude. The territory is where the work needs to happen, the codebase, the real world, its actual constraints. The difference between the map and the territory is what I call unknowns."*

「地图」是你给模型的提示词 / 技能 / 上下文；「疆域」是真正要发生工作的地方——代码库、现实世界、真实约束。**地图与疆域之间的差距，就是未知项。** 只要 Claude 撞上一个 unknown，它就只能按「你大概想要什么」去猜。工作是模型做的越多，它碰到的 unknown 就越多。

![地图不是疆域：地图与疆域之间的差距即未知项](https://cdn.prod.website-files.com/68a44d4040f98a4adf2207b6/6a4be4919e159adcdfa3ec1c_94358c3c.png)
*图示来自 Thariq 原文开篇，呈现「地图 / 疆域 / 未知项」的关系框架。*

### 2. Blind Spot Pass（这个技能的核心动作）

> *"When starting work, one of the most useful things you can do is understand your blind spots. ... you're likely to have a lot of unknown unknowns. You may not know what questions to ask, what good looks like, what historical work has been done, or what potholes to avoid. In these situations, you can ask Claude to help you find your unknown unknowns and explain them to you. I like to use the literal words 'blind spot pass' and 'unknown unknowns.'"*

当你进入代码库里不熟悉的区域、或做没把握的事（比如迭代一个设计），你会有大量 **unknown unknowns**——你甚至不知道该问什么问题、好结果长什么样、历史踩过哪些坑。这篇文章给出的解法，正是让模型替你做一次 **blind spot pass（盲点扫描）**，把那些你没想到的未知项找出来讲给你听。本技能把这套做法固化成了标准工作流与模板。

### 3. unknown knowns（藏在代码库里的隐性规范）

> *"When I'm working in an area with a lot of unknown knowns, involving criteria I only know to define when I see it..."*

还有一种差距是 **unknown knowns**：那些你「看到才认得出来」的隐性标准——已有的错误处理方式、命名规范、架构约定。本技能通过**扫描当前代码库**来发现它们，而不是等你在对话里逐条交代。

> *"reducing and planning for your unknowns is the skill of agentic coding."*

把未知项暴露出来并提前规划，本身就是「agent 协作式编程」这项技能本身。

---

## 它如何工作

`known-unknown` 把上面三件事落成一个四步工作流，对应「盲点扫描 → 可视化 → 引导解答 → 输出规格」：

1. **需求接收与盲点扫描**：读取 `prompts/blindspot.md` 的指令，结合代码库上下文做 blind spot pass，找出 unknown unknowns 与 unknown knowns。
2. **生成交互式 HTML**：以 `templates/unknowns_template.html` 为基础，填入四象限内容，产出一份可视化的未知项扫描报告。
3. **引导用户解答**：在报告底部引导你依次回答「已知的未知」与「未知的未知」中的问题。
4. **输出规格与衔接**：你回答后，生成《需求规格与未知项清单》，并在末尾引导你进入下一关——「确认测验（confirm-quiz）」门禁。

### 四象限模型

| 象限 | 含义 | 文章中的对应 |
|---|---|---|
| 🟢 已知的已知 | 你写在需求里的明确内容（地图里已画出的部分） | 地图本身 |
| 🟡 已知的未知 | 你清楚自己还没弄清、知道该问的问题 | knowns |
| 🟣 未知的已知 | 对你显而易见、但没写下来的隐性规范（扫代码库发现） | unknown knowns |
| 🔴 未知的未知 | 完全没想过的盲点与知识缺口（blind spot pass 发现） | unknown unknowns |

---

## 文件结构

```
known-unknown/
├── SKILL.md              # 技能定义（含触发语义、工作流、衔接说明）
├── prompts/
│   └── blindspot.md      # 盲点扫描提示词模板
└── templates/
    └── unknowns_template.html  # 四象限扫描报告模板
```

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

## 下一步：衔接「确认测验」

扫描完未知项、产出需求规格后，**不要直接动手写代码**。本技能在输出末尾会引导你安装并使用搭档技能
[**确认测验（confirm-quiz）**](https://github.com/mamawe/confirm-quiz)：它在实现前出计划、实现中记偏差、实现后出测验报告，并作为**合并硬门禁**——只有你通过测验，才允许提交/合并。

```bash
git clone https://github.com/mamawe/confirm-quiz.git
cd confirm-quiz && zip -r confirm-quiz.zip .
```

---

## 出处 / 参考

- Thariq Shihipar, *A Field Guide to Claude Fable 5: Finding your Unknowns*, Anthropic Blog,
  https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns
- 原帖：https://x.com/trq212/status/2073100352921215386
- 搭档技能：[确认测验 confirm-quiz](https://github.com/mamawe/confirm-quiz)

> 本文档仅节选上述文章中与「发现未知项（blind spot pass / unknowns）」相关的部分用于说明本技能的设计与工作原理，未搬运全文。配图版权归原作者所有。
