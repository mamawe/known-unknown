### 阶段 01: Intake — 需求接收与边界锁定
# Phase 1: Intake - Understanding Requirements & Defining Scope

本阶段的目标是**在不动手改一行代码前，让用户与模型对需求的理解完全对齐**。不要急于进入细节扫描，先锁死地图和边界。

---

## 📥 输入契约 (Input Contract)
- 用户的原始请求（口头或文字）。
- 代码库当前分支的最新状态。

---

## 🎯 扫描指令 (Scan Instructions)

请以极度专业、严谨的系统架构师姿态，引导用户明确以下 3 类核心边界：

1. **原始意图 (Original Intent)**
   - 用一句话重述用户最核心想解决的问题。
   - 过滤掉无意义的口头修饰，抽取出真正要改变的**实体(Entities)、行为(Behaviors)或流(Flows)**。

2. **在册范围 (In-Scope Boundary)**
   - 本次改动**必须**触碰并修改的代码路径、数据库表、外部 API、微服务边界。
   - 必须提供清晰的代码路径（例如：`src/controllers/payment.js`）。

3. **排除边界 (Out-of-Scope Boundary)**
   - 本次改动**绝对不能**破坏、或者本次需求暂不包含的相关关联系统。
   - 这也是最容易产生 unknown knowns 的地带，提前说清楚哪些属于"已知不需要做的"。

---

## 📝 阶段产出模板 (Output Template)

```markdown
## 1. 需求与边界 (Intake & Scope)
* **原始需求**: {{ INPUT_REQUEST }}
* **核心目标**:
  - [ ] 目标 1: ...
  - [ ] 目标 2: ...
* **在册范围 (IN SCOPE)**:
  - 核心文件/逻辑 1 (路径: `...`)
  - 核心文件/逻辑 2 (路径: `...`)
* **排除范围 (OUT OF SCOPE)**:
  - 关联系统 A (保持现状，不进行修改)
```

---

## 🚦 下一步动作 (Next Action)
一旦范围确认，立即加载 `prompts/02_repo_scan.md` 针对在册范围文件进行代码扫描。
