---
name: life-os
description: Used when user asks to record/search thoughts, conversations with ai, or principles, search knowledge, or update personal state. 
---

# Life OS Skill

## Purpose

Life OS 是个人知识管理系统。

存储三项主要内容：

- **记录 (Thoughts)** - 捕捉瞬间的思考与洞察
- **讨论 (Conversations)** - 与 AI 讨论的各种话题等
- **准则管理 (Principles)** - 记录各阶段的做事原则和方法论

核心功能：
- **简单即强大** - 使用 Markdown 格式，便于版本管理和人类阅读
- **语义检索** - 可以通过 bash 快速检索内容


### 数据存储位置

所有数据存储在 `~/.life-os/` 目录下：

```
~/.life-os/
  ├── thoughts/         # 按日期存储的想法记录
  ├── conversations/    # 带编号的讨论记录
  ├── principles/       # 准则文件
  ├── current-state.md  # 用户当前状态文件
  └── tags-index.md     # 全局标签索引（每次记录新的内容后，AI 需要自动此文件）
```

## Workflow

### 0. 同步远端数据

任何读/写操作前，首先在 ~/.life-os/ 目录下执行  `git pull`

### 1. 判断用户意图
分析用户输入，判断是以下哪种操作：
- 记录想法 → Write Thought
- 记录讨论 → Write Conversation
- 记录准则 → Write Principle
- 检索查询 → Search
- 更新状态 → Update State

### 2. 执行对应操作
详见各 Operations 的具体流程

### 3. 推送到远端

修改 ~/.life-os/ 中任何文件后，将其推送到远端

## Operations

### Write Thought (记录想法)

**触发条件:**
- "记个想法 [内容]"
- "有个想法 [内容]"
- "记录一下 [内容]"

**流程:**

1. **获取当前日期**
   ```bash
   TODAY=$(date "+%Y-%m-%d")
   FILE_PATH="$HOME/.life-os/thoughts/$TODAY.md"
   ```

2. **确保目录存在**
   ```bash
   mkdir -p "$HOME/.life-os/thoughts"
   ```

3. **读取现有文件**（如果存在）
   - 使用 Read 工具读取现有内容
   - 保留文件头部的 frontmatter（如果有）

4. **生成新的想法条目**
   - 从用户输入提取标题（使用第一句话或生成简洁标题）
   - 自动推断相关 tags 和 keywords
   - 添加时间戳
   ```markdown
   ---
   created: 2026-02-26 14:30
   tags: [从用户输入提取或询问]
   keywords: [从用户输入提取或询问]
   ---

   ## [想法标题或第一句话]

   [用户输入的内容]
   ```

5. **追加到文件**
   - 如果文件存在，在末尾追加新条目（保留两个空行分隔）
   - 如果文件不存在，创建新文件

6. **更新 tags-index.md**
   - 提取新的 tags
   - 添加到全局索引文件
   - 使用 Update Index 操作

**输出示例:**
```
✅ 已记录想法到 ~/.life-os/thoughts/2026-02-26.md
```

### Write Conversation (记录讨论)

**触发条件:**
- "将讨论记录下来"
- "将对话记录下来"
- "记录当前对话"

**流程:**

1. **确保目录存在**
   ```bash
   mkdir -p "$HOME/.life-os/conversations"
   ```

2. **获取下一个编号**
   ```bash
   cd ~/.life-os/conversations
   max_num=$(ls *.md 2>/dev/null | sed 's/-.*//' | sort -n | tail -1)
   next_num=$((max_num + 1))
   printf "%03d" $next_num
   ```

3. **生成文件名**
   - 格式：`{编号}-{主题}.md`
   - 从会话中提取主题作为文件名（去除特殊字符）
   - 文件路径：`~/.life-os/conversations/{编号}-{主题}.md`

4. **创建讨论文件**
   ```markdown
   ---
   title: [讨论主题]
   created: 2026-02-26
   tags: [讨论中提取]
   keywords: [讨论中提取]
   ---

   # [讨论主题]

   ## 问题背景
   [用户描述]

   ## 讨论要点
   - [要点1]
   - [要点2]

   ## 结论/行动
   [讨论结论]

   ----------------- 讨论原文 -----------------
   [绝对不经 AI 总结的讨论原文]

   ```

5. **更新 tags-index.md**
   - 使用 Update Index 操作

6. **询问是否更新状态**
   ```
   讨论已记录。是否需要更新当前状态？
   ```

**输出示例:**
```
📝 讨论已记录到 ~/.life-os/conversations/001-是否接受新工作.md
```

### Write Principle (记录准则)

**触发条件:**
- "记个原则：[标题] [内容]"
- "这是我做事的准则：[标题] [内容]"
- "把这个记为准则：[标题]"

**流程:**

1. **确保目录存在**
   ```bash
   mkdir -p "$HOME/.life-os/principles"
   ```

2. **提取准则标题**
   - 从用户输入提取标题
   - 如果不明确，询问用户
   - 标题应简洁明了，便于后续检索

3. **生成文件名**
   - 使用标题作为文件名
   - 去除特殊字符，保留中文、英文、数字
   - 格式：`~/.life-os/principles/{标题}.md`

4. **创建准则文件**
   ```markdown
   ---
   title: [准则标题]
   created: 2026-02-26
   tags: [从内容提取]
   keywords: [从内容提取]
   ---

   # [准则标题]

   ## 核心思想
   [内容摘要]

   ## 使用准则
   1. [准则点1]
   2. [准则点2]

   ## 适用场景
   - [场景1]
   - [场景2]
   ```

5. **更新 tags-index.md**
   - 使用 Update Index 操作
   - 提取新的 tags
   - 添加到索引文件


**输出示例:**
```
✅ 准则已记录到 ~/.life-os/principles/决策四象限法.md
```

### Search (检索)

**触发条件:**
- "我的原则是什么" / "按照准则该怎么做"
- "我之前是怎么想的" / "之前讨论过 xxx 吗"
- "相关准则" / "相关讨论"
- "搜索 [关键词]" / "查找 [主题]"
- "关于 xxx 的记录"

**流程:**

1. **读取当前状态**
   ```bash
   STATE_FILE="$HOME/.life-os/current-state.md"
   if [ -f "$STATE_FILE" ]; then
       current_state=$(cat "$STATE_FILE")
   fi
   ```
   - 获取当前阶段、关注点和活跃准则
   - 提取当前主题标签作为相关性权重

2. **提取关键词**
   - 从用户输入中提取核心搜索词
   - 扩展同义词和相关术语
   - 结合 current-state.md 中的 tags 增强搜索
   - 识别搜索意图（准则、讨论、想法或全部）

3. **并行搜索**
   - 使用 Grep 工具同时搜索三个数据源
   - 搜索 principles/、conversations/、thoughts/
   - 搜索范围：标题、tags、keywords 和正文内容
   - 搜索模式：关键词匹配（支持中文）

4. **结果排序和分组**
   - current-state.md 中引用的准则优先
   - 标签匹配度高的文件优先
   - 创建时间近的优先（可选）
   - 按文件类型分组：Principles > Conversations > Thoughts

5. **格式化输出**
   - 使用清晰的结构展示结果
   - 包含文件路径和匹配片段
   - 提供快速访问链接
   - 显示当前上下文信息

**输出示例:**
```
🔍 搜索"决策"的结果

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📌 准则 (Principles)

✨ 决策四象限法
📁 ~/.life-os/principles/决策四象限法.md
匹配：标题、tags、keywords
核心思想：做决策时，将事项按"紧急度"和"重要度"分类...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💬 讨论 (Conversations)

🔹 001-是否接受新工作
📁 ~/.life-os/conversations/001-是否接受新工作.md
匹配：keywords 包含"决策"
结论：倾向于接受，主要是团队匹配度高

🔹 005-职业选择三要素
📁 ~/.life-os/conversations/005-职业选择三要素.md
匹配：tags 包含"决策"
结论：核心考量因素是成长空间、薪资、团队文化

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💭 想法 (Thoughts)

📝 2026-02-26 - 关于决策的思考
📁 ~/.life-os/thoughts/2026-02-26.md
匹配：正文包含"决策"
核心观点：决策不是为了完美，而是为了前进

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 当前上下文：职业转型期 - 正在评估多个工作机会
```

### Update State (更新状态)

**触发条件:**
- "更新状态：[内容]"
- "当前状态" / "我现在在关注什么"
- 讨论结束后用户确认更新状态

**流程:**

1. **确保目录和文件存在**
   ```bash
   mkdir -p "$HOME/.life-os"
   STATE_FILE="$HOME/.life-os/current-state.md"
   ```

2. **读取现有状态**
   - 使用 Read 工具读取现有状态文件
   - 如果文件不存在，使用默认模板创建

3. **解析更新内容**
   - 从用户输入中提取要更新的字段
   - 识别以下字段类型：
     - `phase` - 当前阶段
     - `focus` - 当前关注点
     - `active_principles` - 活跃的准则
     - `current_topics` - 当前话题
     - `ongoing_projects` - 进行中的项目
     - `pending_questions` - 待讨论问题

4. **更新文件**
   - 合并现有状态和新更新
   - 保留未修改的字段
   - 更新 `last_updated` 时间戳为当前时间
   - 保持 frontmatter 格式一致

5. **确认更新**
   - 使用 Write 工具保存更新后的文件
   - 向用户展示更新后的状态

**输出示例:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 当前状态 (Current State)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 阶段 (Phase)
职业转型期 - 正在评估多个工作机会

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📌 当前关注点 (Focus)
评估公司 A 和公司 B 的 offer，重点关注团队文化和成长空间

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📚 活跃准则 (Active Principles)
• [[决策四象限法]] - 用于评估各个机会
• [[职业选择三要素]] - 核心考量因素

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 当前话题 (Current Topics)
• 职业选择
• 薪资谈判
• 团队文化评估

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 进行中的项目 (Ongoing Projects)
• [ ] 评估公司 A 的 offer（进行中）
• [ ] 与公司 B 面试（待安排）

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

❓ 待讨论问题 (Pending Questions)
1. 如何评估团队文化？
2. 薪资谈判的底线应该是多少？

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔗 相关记录 (Related Records)
• [[001-是否接受新工作]]
• [[002-薪资谈判策略讨论]]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ 状态已更新 | 最后更新: 2026-02-26 18:30
```

### Update Index (更新索引)

**触发时机:**
- 在执行 Write Thought、Write Conversation、Write Principle 操作后自动触发
- 用户明确要求更新索引时
- 索引文件不存在或损坏时

**流程:**

1. **确保所有目录存在**
   ```bash
   mkdir -p "$HOME/.life-os"/{thoughts,conversations,principles}
   ```

2. **扫描所有文件**
   - 使用 Glob 工具扫描所有数据目录
   - 递归查找所有 .md 文件
   - 提取每个文件的 frontmatter
   - 收集所有 tags 和对应文件

3. **构建索引结构**
   - 按标签名分组
   - 记录文件路径、类型和标题
   - 按文件类型排序（Principles > Conversations > Thoughts）
   - 统计文件总数和标签总数

4. **写入索引文件**
   - 使用 Write 工具写入 `~/.life-os/tags-index.md`
   - 包含 last_updated 时间戳
   - 包含统计信息（total_files, total_tags）
   - 格式化展示每个标签下的文件列表


**调用方式:**
- 自动调用：在各 CRUD 操作完成后自动执行
- 手动调用：
  - "更新索引" / "重建索引"
  - "刷新标签索引"
  - " regenerate index"

**输出示例:**
```
🔄 正在更新索引...
📁 扫描目录: ~/.life-os/thoughts, conversations, principles
📋 发现 12 个文件，8 个标签
✅ 索引已更新到 ~/.life-os/tags-index.md
```

## File Formats

### 通用 Frontmatter（所有文件）

```yaml
---
title: xxx
created: 2026-02-26
tags: [xxx, yyy]
keywords: [xxx, yyy, zzz]
---
```

### Thoughts 格式

```markdown
---
created: 2026-02-26 16:30
tags: [ai, programming]
keywords: [ai辅助, 代码质量, 效率]
---

## 关于 AI 辅助编程的思考

今天在使用 Claude Code 时发现...

### 核心观点
1. AI 不是替代，是增强
2. 关键是学会提问

> 需要进一步验证这个观点
```

### Conversations 格式

```markdown
---
title: 是否接受新工作
created: 2026-02-26
tags: [职业, 决策]
keywords: [offer, 薪资, 成长]
---

# 是否接受新工作的讨论

## 问题背景
收到 xx 公司的 offer，在纠结...

## 讨论要点
- 薪资涨幅 30%，但...
- 团队氛围...
- 成长空间...

## 结论/行动
倾向于接受，主要是团队匹配度高

下一步：联系前员工了解内部情况
```

### Principles 格式

```markdown
---
title: 决策四象限法
created: 2026-02-26
tags: [决策, 方法论]
keywords: [四象限, 优先级, 紧急重要]
---

# 决策四象限法

## 核心思想

做决策时，将事项按"紧急度"和"重要度"分类...

## 使用准则

1. **重要且紧急**：立即决策
2. ...

## 适用场景
- 职业选择
- 项目优先级
```

### Current State 格式

```markdown
---
last_updated: 2026-02-26
phase: 职业转型期
focus: 评估新工作机会
---

# 当前状态

## 阶段
职业转型期 - 正在评估多个工作机会

## 当前关注的行为准则
- [[决策四象限法]] - 用于评估各个机会
- [[职业选择三要素]] - 核心考量因素

## 当前主题/话题
- 职业选择
- 薪资谈判
- 团队文化评估

## 正在进行的项目
- [ ] 评估公司 A 的 offer（进行中）
- [ ] 与公司 B 面试（待安排）

## 待讨论的问题
1. 如何评估团队文化？
2. 薪资谈判的底线应该是多少？

## 相关记录
- [[001-是否接受新工作]]
- [[002-薪资谈判策略讨论]]
```

### Tags Index 格式

`tags-index.md` 是自动生成的全局索引文件，按标签分组列出所有文件：

```markdown
---
last_updated: 2026-02-26
---

# 标签索引

## 职业决策
- [[001-是否接受新工作]]
- [[决策四象限法]]
- [[职业选择三要素]]

## AI 辅助
- [[2026-02-26]] (thoughts)
- [[关于AI辅助编程的思考]] (conversations)

## 方法论
- [[决策四象限法]]
- [[优先级管理法则]]
```

## Examples

### Example 1: 记录想法

```
User: 记个想法，我觉得AI辅助编程能提高效率，但关键是学会如何提问

Actions:
1. 获取当前日期: 2026-02-26
2. 创建/追加到 ~/.life-os/thoughts/2026-02-26.md
3. 写入想法内容，添加相关标签和关键词
4. 更新 tags-index.md
```

### Example 2: 记录讨论

```
User: 记录上面的讨论

Actions:
1. 扫描 conversations/ 目录，获取最大编号（如 002）
2. 创建 ~/.life-os/conversations/003-是否接受新工作.md
3. 记录讨论内容
4. 更新 tags-index.md
5. 询问: "是否需要更新当前状态？"
```

### Example 3: 搜索准则

```
User: 按照我的准则，该如何做决策？

Actions:
1. 读取 current-state.md 获取当前上下文
2. 搜索 principles/ 目录中包含"决策"的文件
3. 展示相关准则，优先显示 current-state.md 中引用的准则
```

### Example 4: 更新状态

```
User: 更新状态：我现在正在评估两个工作机会

Actions:
1. 读取 current-state.md
2. 更新 phase 和 focus 字段
3. 保存更新后的文件
```

## Implementation Notes

**目录结构:**
- 使用 `~/.life-os/` 作为根目录，确保从任何工作目录都可访问
- 各子目录按功能划分：thoughts/, conversations/, principles/

**文件命名:**
- Thoughts: 按日期命名，`YYYY-MM-DD.md`
- Conversations: 自动编号，`{编号}-{标题}.md`（如 001-是否接受新工作.md）
- Principles: 按标题命名，使用中文文件名

**自动编号:**
- 扫描 conversations/ 目录获取最大编号
- 新文件编号 = 最大编号 + 1
- 格式：`001-`, `002-`, ...

**索引维护:**
- 每次 CRUD 后更新 `tags-index.md`
- 按标签分组列出所有文件
- 包含 last_updated 字段

**状态更新时机:**
- 每次讨论后，询问用户"是否需要更新当前状态？"
- 用户可主动说"更新状态：xxx"

**检索策略:**
- 优先展示 current-state.md 中相关的内容
- 使用 grep 进行关键词搜索
- 按相关性排序展示结果

**Frontmatter 规范:**
- 所有文件都应包含 frontmatter
- created 字段使用 `YYYY-MM-DD HH:MM` 或 `YYYY-MM-DD` 格式
- tags 和 keywords 使用数组格式

## 错误处理和边界情况

**目录不存在：**
- 所有操作前都应确保目录存在
- 使用 `mkdir -p` 自动创建所需目录

**文件名冲突：**
- Conversations 使用自动编号避免冲突
- Principles 和 Thoughts 使用明确的命名规则

**空输入处理：**
- 当用户输入不明确时，主动询问澄清
- 提供合理的默认值供用户确认

**索引损坏：**
- 检测索引文件是否存在或损坏
- 提供重建索引的选项

## 最佳实践

**标签选择：**
- 使用简洁、描述性的标签
- 保持标签体系的一致性
- 避免过度细分标签

**关键词提取：**
- 从内容中提取核心概念
- 包含同义词和相关术语
- 便于后续检索

**状态更新频率：**
- 在重要讨论后及时更新
- 阶段变化时更新 phase
- 保持当前关注点的准确性

**检索技巧：**
- 使用具体的关键词而非泛泛的词汇
- 利用当前上下文提高检索精度
- 关注标签匹配度
