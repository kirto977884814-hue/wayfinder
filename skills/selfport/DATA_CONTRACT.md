# Profile Skill 数据契约（DATA_CONTRACT）

> 这是 **Profile Skill（生产者）** 与 **下游技能（消费者，如 Exploration / Teach）** 之间的稳定接口。
> 消费者只需读本文件，即可理解画像数据的格式与位置，**无需了解 Profile 的访谈逻辑**。
> 生产者必须严格遵守本契约写入；消费者默认只读，需要更新时调用 Profile。

---

## 契约版本

- `schema_version`: **"1.0"**
- 后续若 schema 变更，bump 版本号并在本文件顶部「变更说明」标注。

---

## 1. 数据位置

画像数据存在用户本地一个 `PROFILE/` 文件夹：

| 平台 | 默认路径 |
|------|---------|
| Unix / macOS | `~/.personal-profile/PROFILE/` |
| Windows | `C:\Users\<用户名>\.personal-profile\PROFILE\` |

**可配置**：设置环境变量 `PROFILE_DATA_DIR` 可覆盖默认位置（指向 `PROFILE/` 的**父目录**，即 `.personal-profile/`）。

> 路径刻意中性（不带 profile-skill 品牌），方便融入其他技能后双方共用，互不尴尬。

---

## 2. 目录结构

```
PROFILE/
├── LIFE_TIMELINE.md        # Session 1 产出
├── INTEREST_SIGNALS.md     # Session 2 产出
├── VALUES.md               # Session 3 产出
├── STRENGTHS.md            # Session 4 产出
├── CURRENT_STAGE.md        # Session 5 产出
├── RELATIONSHIPS.md        # Session 6 产出
├── LEARNING_HISTORY.md     # 非访谈产物，由 Exploration 反馈 / 用户补充累积
└── PROFILE.md              # 汇总文件（含 frontmatter，消费者主入口）
```

---

## 3. PROFILE.md 格式（消费者主入口）

分两层：**YAML frontmatter**（结构化，精确取用）+ **Markdown 正文**（叙事，深度理解）。

### frontmatter schema

```yaml
---
schema_version: "1.0"          # 契约版本
version: 2026-06-21            # 最后更新日期（YYYY-MM-DD）
progress: [1, 2, 3]            # 已完成的 Session 编号（消费者可读）
interest_keywords: [...]       # 兴趣关键词
value_keywords: [...]          # 价值观关键词
strength_keywords: [...]       # 优势关键词
current_stage: "..."           # 当前阶段
cognitive_style: "..."         # 认知风格
learning_style: "..."          # 学习风格
explore_preferences:           # 探索偏好
  prefer: [...]                #   偏好的内容形式
  avoid: [...]                 #   回避的内容形式
---
```

### 字段类型一览

| 字段 | 类型 | 说明 |
|------|------|------|
| `schema_version` | string | 契约版本，当前 `"1.0"` |
| `version` | date | 最后更新日期 |
| `progress` | list[int] | 已完成 Session 编号；`[1,2,3,4,5,6]` 表示画像完整 |
| `interest_keywords` | list[string] | 兴趣关键词 |
| `value_keywords` | list[string] | 价值观关键词 |
| `strength_keywords` | list[string] | 优势关键词 |
| `current_stage` | string | 当前人生阶段 |
| `cognitive_style` | string | 认知风格 |
| `learning_style` | string | 学习风格 |
| `explore_preferences` | object | `{prefer: [...], avoid: [...]}` |

### 正文结构

基础信息 / 成长经历摘要 / 兴趣画像 / 价值观画像 / 能力画像 / 当前阶段 / 关系画像。
底部维护 **变更记录** 区。

---

## 4. 画像文件两层结构（LIFE_TIMELINE / INTEREST_SIGNALS / 等）

每个画像文件分两层：

- **结论层**：关键词、标签 —— 对应 frontmatter 的结构化字段，供消费者精确调用。
- **证据层**：每个关键词下面挂着对应的故事，每条带 **来源**（`Session X · YYYY-MM-DD`）。

> 关键词回答「是什么」，故事回答「为什么、有多深」。
> 骨架示例见 `templates/*.md`。

---

## 5. 更新约定

不同字段更新方式不同，消费者读取时需理解其语义：

| 类型 | 文件 | 行为 |
|------|------|------|
| **追加型**（只增不改） | LIFE_TIMELINE, INTEREST_SIGNALS, LEARNING_HISTORY | 新内容加在后面 |
| **状态型**（替换当前值，归档旧值） | VALUES, CURRENT_STAGE | 当前值会变；旧值进变更记录 |
| **深化型**（原地精化） | STRENGTHS, RELATIONSHIPS | 描述变得更准确 |

### 变更记录

PROFILE.md 底部维护，格式：

```
YYYY-MM-DD  字段: 旧值 → 新值（原因）
```

累积起来即用户的「成长轨迹」。每次更新时 frontmatter 的 `version` 同步刷新。

---

## 6. 消费者使用指引（给 Exploration / Teach）

1. **入口**：读 `PROFILE.md`。先看 frontmatter 判断画像是否完整（`progress` 是否含全部 6 个 Session）。
2. **精确取用**：推荐 / 匹配逻辑直接读 frontmatter 的关键词字段。
3. **深度理解**：需要「为什么」时，读对应画像文件的证据层，或 PROFILE.md 正文。
4. **只读原则**：消费者**不要直接改**画像文件。需要更新（如发现用户状态变了），调用 Profile 的入口触发更新流程，由 Profile 负责访谈与写入。
5. **回写学习（唯一例外）**：用户通过 Exploration 学到 / 探索了新东西，可写入 `LEARNING_HISTORY.md`——这是**唯一允许消费者写入**的文件，作为反馈闭环。

---

## 变更说明

- **1.0**（初版）：确立目录结构、frontmatter schema、两层结构、三种更新法、消费者指引。
