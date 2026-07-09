# 方法论来源：Thariq Shihipar《Finding your Unknowns》
# METHODOLOGY.md

本文件完整保存 `known-unknown` 技能的方法论来源与设计哲学。正文节选自 Anthropic 工程师 Thariq Shihipar 的文章 [A Field Guide to Claude Fable 5: Finding your Unknowns](https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns)（原帖：[@trq212](https://x.com/trq212/status/2073100352921215386)），仅节选与「发现未知项」直接相关的部分用于说明本技能的设计原理，配图版权归原作者所有。

---

## 1. 地图不是疆域（为什么要做盲点扫描）

> "When working with Claude Code, I'm often reminded of the difference between the map and the territory. The map is what you give Claude — your prompts, your context, your instructions. The territory is the actual codebase, with all its history, edge cases, and implicit rules."

**「地图」** 是你给模型的提示词 / 技能 / 上下文；**「疆域」** 是真正要发生工作的地方——代码库、现实世界、真实约束。

**地图与疆域之间的差距，就是未知项（Unknowns）。** 只要 Claude 撞上一个 unknown，它就只能按「你大概想要什么」去猜。你委托模型做的工作越多，它碰到的 unknown 就越多。

*图示（来自 Thariq 原文开篇）*：呈现「地图 / 疆域 / 未知项」的关系框架——模型在「地图边缘」之外盲区行事，是 agentic coding 最核心的风险来源。

---

## 2. 四象限框架（如何分类未知项）

Thariq 在文中引入了经典的「已知/未知」四象限矩阵（源自 Johari Window 认知模型），并将其适配到 agentic coding 的上下文中：

| 象限 | 用户视角 | 模型视角 | 如何处理 |
|------|---------|---------|---------|
| **🟢 已知的已知** (Known Knowns) | 明确知道 | 被告知 | 写进需求文档，直接实现 |
| **🟡 已知的未知** (Known Unknowns) | 知道自己不知道 | 未被告知 | 引导用户回答，澄清后实现 |
| **🟣 未知的已知** (Unknown Knowns) | 隐性知道但未说出 | 从代码推断 | 扫描代码库发现后呈现给用户确认 |
| **🔴 未知的未知** (Unknown Unknowns) | 完全不知道 | 无法知道（除非主动推演）| Blind Spot Pass 主动发现 |

**Unknown Unknowns 是最危险的**：不仅用户不知道答案，他们甚至不知道该问什么问题。

---

## 3. Blind Spot Pass（核心动作）

> "When starting work, one of the most useful things you can do is understand your blind spots. If you're working in an area of the codebase you haven't touched, or you're doing something you're not confident about (like iterating on a design), you'll have a lot of unknown unknowns — things you don't even know to ask about, things you don't know would be good, historical gotchas you don't know about."

Thariq 给出的解法：**让模型在动手前替你做一次 Blind Spot Pass（盲点扫描）**，把那些你没想到的未知项找出来讲给你听，然后你补充信息，再让模型继续。

本技能把这套做法固化为六阶段工作流（详见 `SKILL.md`），核心是：
1. 模型先主动读取代码库，发现 Unknown Knowns（隐性规范）。
2. 模型再做防守性灾难推演，产出 Unknown Unknowns（黑天鹅风险）。
3. 用户逐条解答，答案写回规格文件。

---

## 4. Unknown Knowns（藏在代码库里的隐性规范）

> "When I'm working in an area with a lot of unknown knowns, involving criteria I only know to define when I see them — like code quality standards, design quality, or feature interactions — I ask Claude to do a blind spot pass before we start working."

还有一种差距是 **Unknown Knowns**：那些你「看到才认得出来」的隐性标准——已有的错误处理方式、命名规范、架构约定。

这类信息存在于代码库里，但没有人写在文档中。模型如果不主动去读，就会按自己的默认行为行事，产出与项目风格截然不同的代码。本技能通过「扫描当前代码库」来发现它们，而不是等你在对话里逐条交代。

---

## 5. 核心结论

> "Reducing and planning for your unknowns is the skill of agentic coding."

**把未知项暴露出来并提前规划，本身就是「agentic coding」这项技能本身。**

`known-unknown` 把这句话做成了一个可被机器执行的契约：
- 触发语义 → SKILL.md
- 扫描分类学 → taxonomy/scan_dimensions.yaml
- 结构化产物 → templates/spec.schema.json
- 可视化交互 → templates/unknowns_report.html
- 下游门禁 → confirm-quiz

---

*本文档仅节选上述文章中与「发现未知项」相关的部分，未搬运全文。如需阅读完整原文，请访问上方链接。*
