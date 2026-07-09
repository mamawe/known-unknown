### 阶段 02: Repo Scan — 代码库隐性规范扫描（发现 Unknown Knowns）
# Phase 2: Repo Scan - Identifying Hidden Conventions

本阶段的目标是沿 12 个扫描维度寻找**藏在代码库里、对用户显而易见但未明文写出、而模型极其容易犯错的隐性规范与既有约定（Unknown Knowns）**。

---

## 🎯 扫描维度与指令 (Scanning Taxonomy & Instructions)

请使用 `taxonomy/scan_dimensions.yaml` 中定义的 **12 类维度** 对 Intake 阶段锁定的 `IN SCOPE` 目录和文件进行**深度语义扫描**。

### 🔍 重点扫雷目标 (Key Signals to Identify):

1. **既有异常与报错风格**
   - 检查项目内是否统一抛出特定封装异常（如 `AppException`），还是直接抛出裸 `Error`。
   - 检查是否有全局错误拦截中间件对特定 error code 有隐性依赖。

2. **命名与设计偏好**
   - 观察周围同类文件的命名（如 Adapter 模式还是 Strategy 模式）。
   - 是习惯用 DTO 传递参数，还是裸对象、元组或 Map？

3. **依赖与底层方法**
   - 观察是否偏好内置工具库（如 lodash, rambda）或项目内已封装的辅助函数，防止重复造轮子。

4. **状态与软删除约束**
   - 数据库实体中是否默认带 `is_deleted`，改动是否需要联动更新此类字段。

---

## 📝 阶段产出模板 (Output Template)

在分析结束后，将扫描到的 unknown known 规整为如下格式：

```markdown
### 🟣 未知的已知 (Unknown Knowns) - 发现的隐性规范

#### [U-XXX] 发现的隐性规范描述
* **扫描维度**: {{ DIMENSION_ID }} ({{ DIMENSION_NAME }})
* **论证与证据**: 
  - **证据源**: `path/to/file.js:line` 或近期的 commit hash
  - **原因分析**: 观察到此处代码使用的是 `...` 规范，因此我们在实现新逻辑时也必须采用此格式。
* **风险评估**: Impact: {{ IMPACT }} | Likelihood: {{ LIKELIHOOD }} | Reversibility: {{ REVERSIBILITY }} | Score: {{ SCORE }}
```

---

## 🚦 下一步动作 (Next Action)
完成隐性规范扫描后，加载 `prompts/03_blindspot_pass.md` 寻找需求和技术路线中那些"完全没有防备"的盲点。
