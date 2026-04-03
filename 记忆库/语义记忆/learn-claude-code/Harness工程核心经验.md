# Agent Harness 工程指南
> 来源: Learn Claude Code 项目深度学习
> 标签: #Agent #Harness工程 #智能体开发
> 版本: v2.0 | 2026-03-31

---

## 核心哲学：3 句话

> **Agent 是模型，不是代码。代码是 Harness，不是 Intelligence。**

> **模型决定行动。Harness 执行。**

> **最好的 Harness 代码是无聊的。魔法在模型里，不在代码里。**

---

## 第一性原理

### Agent 是什么

Agent 是一个**神经网络**——经过数十亿次梯度更新训练出来的、学会感知环境、推理目标、采取行动的模型。

**历史已经证明**：

| 年份 | 项目 | 证明什么 |
|------|------|---------|
| 2013 | DeepMind DQN | 单一神经网络，无规则编码，能学会玩 Atari |
| 2019 | OpenAI Five | 自我对弈能学会团队协作 |
| 2019 | AlphaStar | 实时决策，无脚本策略 |
| 2024-25 | Claude/GPT | 通用任务，相同架构 |

**共同真理**：Agent 永远是模型本身。

### Agent 不是什么

❌ **提示词水管工**——拖拽式工作流、提示链编排、用 if-else 分支串 LLM API

❌ **GOFAI 还魂**——符号规则系统喷了一层 LLM 的漆

**本质区别**：

```
真正的 Agent                    提示词水管工
===============                =============
模型决定行动                    程序员预设流程
从经验学习                      硬编码规则
泛化到新场景                    遇到边界就崩溃
Harness 提供环境                胶水代码堆砌
```

### Harness 的精确定义

```
Harness = Tools + Knowledge + Observation + Action Interfaces + Permissions

    Tools:          Agent 的手——文件读写、Shell、网络、数据库
    Knowledge:      Agent 的知识——产品文档、API 规范、领域资料
    Observation:    Agent 的眼睛——git diff、错误日志、浏览器状态
    Action:        Agent 的执行——CLI 命令、API 调用、UI 交互
    Permissions:    Agent 的边界——沙箱隔离、审批流程
```

**核心关系**：

- 模型是驾驶者。Harness 是载具。
- 模型做决策。Harness 执行。
- 模型做推理。Harness 提供上下文。

---

## 设计思维：构建 Agent 前的 5 问

在写任何代码前，先问自己：

### 1. 这个 Agent 要完成什么？

明确**领域**和**任务类型**。

```
你的 Agent 是：
  - 编程助手？→ 需要 IDE/终端/文件系统
  - 科研伙伴？→ 需要文献检索/数据分析
  - 客服？→ 需要对话/工单/知识库
  - 运营助手？→ 需要数据分析/内容生成
```

### 2. 它需要什么能力（Tools）？

**从 3-5 个开始**。添加新能力的标准：模型**反复失败**因为缺少某个能力。

```
工具设计原则：
- 原子化：每个工具做一件事
- 可组合：工具之间能协作
- 描述清晰：模型知道何时用什么
```

### 3. 知识从哪里来（Knowledge）？

知识决定 Agent 是**通用**还是**领域专家**。

```
知识注入方式：
- System Prompt：始终可用的全局知识（少放！）
- Skill 文件：按需加载的专项知识（推荐）
- 工具返回：在执行中注入上下文
```

### 4. 如何跟踪进度（Planning）？

**单次会话**：TodoWrite（内存中，会话结束丢失）

**跨会话**：Task System（磁盘持久化，有依赖图）

### 5. 什么时候需要多个 Agent（Team）？

当任务**规模**或**多样性**超出单个 Agent 能力时。

```
信号：需要团队的征兆
- 同时执行多个独立任务
- 任务需要不同专业知识
- 需要并行处理加速
```

---

## Harness 工程 12 原则

每条原则对应一个 Session，渐进复杂：

### s01 · One loop & Bash is all you need
**最小循环 + 一个工具 = Agent**

核心只有一个 loop，模型通过 `stop_reason` 控制何时调用工具、何时停止。

### s02 · Adding a tool means adding one handler
**加工具就是加一个 handler，循环代码不变**

Dispatch Map 模式：`name -> handler`。加新工具不影响循环结构。

### s03 · An agent without a plan drifts
**无规划的 Agent 会迷失**

先列步骤再执行，完成率翻倍。TodoWrite 是单会话规划，nag 提醒保持专注。

### s04 · Break big tasks down; each subtask gets a clean context
**子任务需要干净上下文**

子 Agent 用独立的 messages[]，主 Agent 只收到摘要。隔离噪声，保持主上下文干净。

### s05 · Load knowledge when you need it, not upfront
**知识要按需加载，不要前置**

System Prompt 只放技能名称（Layer 1），完整内容在 Skill 文件中（Layer 2）。

### s06 · Context will fill up; you need a way to make room
**上下文会满，需要清理机制**

三层压缩：microcompact（每轮）→ auto_compact（超阈值）→ manual compact（手动）。

**关键**：read_file 结果不压缩（重读比记忆更高效）。

### s07 · Break big goals into small tasks, order them, persist to disk
**任务要持久化，依赖要管理**

Task System = 文件型 DAG。`blockedBy` 单向依赖，完成后自动解锁。

### s08 · Run slow operations in the background; the agent keeps thinking
**慢操作放后台，Agent 继续思考**

守护线程执行，通知队列返回结果。Fire and forget。

### s09 · When the task is too big for one, delegate to teammates
**单个 Agent 扛不住时，委托给队友**

持久化队友 + 异步邮箱（JSONL）。队友独立运行，通过邮箱通信。

### s10 · Teammates need shared communication rules
**队友之间要有统一的沟通规矩**

Request-Response 模式驱动所有协商。相同 FSM：pending → approved/rejected。

### s11 · Teammates scan the board and claim tasks themselves
**队友自己扫描任务板，认领任务**

Idle 轮询 + 自动认领。无需领导分配，减少协调成本。

### s12 · Each works in its own directory, no interference
**各 Agent 在独立目录工作，互不干扰**

Worktree 隔离 + Task 协调。Task 绑定 Goal，Worktree 绑定 Directory。

---

## 工具设计规范

### 工具的本质

工具是 **Agent 的手**——它能执行的动作。

```
工具回答：Agent 能做什么？
```

### 描述怎么写

描述是 Agent 知道**何时用**这个工具的唯一依据。

```python
# ❌ 差：太模糊
{"name": "search", "description": "Search for information"}

# ✅ 好：说明能力和时机
{
    "name": "search_paper",
    "description": "Search academic papers via SerpAPI. Use when user asks about research papers, literature review, or academic publications."
}
```

**描述公式**：
```
[工具名]：做什么 + 何时用
```

### 数量如何定

**从 3-5 个开始**。添加标准：模型反复因缺少某个能力而失败。

```
反模式：
- 一次设计 20+ 工具
- 工具职责重叠
- 过早优化

正确做法：
- 从最小集合开始
- 观察模型失败模式
- 按需添加
```

### 常见工具类型

| 类型 | 工具名 | 触发场景 |
|------|--------|---------|
| 文件操作 | read, write, edit | 读写文件、创建代码 |
| Shell | bash | git、npm、python、编译 |
| 网络 | http_request | API 调用、爬虫 |
| 搜索 | search | 搜索信息、论文、代码 |
| 业务 | send_email, create_task | 具体业务操作 |

---

## 知识设计规范

### 知识是 Agent 的 Expertise

```
知识回答：Agent 知道怎么做？
```

### 两层注入架构

```
Layer 1（廉价）：System Prompt 中的技能名称
  → 模型知道有什么可用

Layer 2（按需）：Skill 文件完整内容
  → 模型需要时加载
```

### Skill 文件格式

```yaml
---
name: pdf_processing
description: Process PDF files and extract content
trigger: pdf, document, extract
---

# PDF Processing Skill

## When to Use
- User asks to read/analyze a PDF
- Extracting content from academic papers

## Steps
1. Use `pdfplumber` to open the PDF
2. Extract text page by page
3. Handle tables with `extract_table()`

## Tips
- First page often contains abstract
- Check for references section
```

### 知识加载时机

```
❌ 反模式：所有知识塞进 System Prompt
   → 上下文膨胀
   → 模型被噪声淹没

✓ 正确做法：按需加载
   → System Prompt 只列技能名称
   → 模型知道有什么可用
   → 遇到需要时主动加载
```

---

## 上下文管理原则

### 上下文是 Agent 的 Memory

```
上下文回答：已经发生了什么？
```

### 三层压缩策略

| 层级 | 触发 | 机制 | 保留 |
|------|------|------|------|
| Microcompact | 每轮 | 清理旧 tool_result | 最近 3 个 + read_file |
| Auto-compact | 超阈值 | 保存+摘要 | 最后 8 万字符 + 摘要 |
| Manual compact | 手动调用 | 立即压缩 | 同上 |

### 关键设计决策

```
read_file 结果不压缩：
- 文件内容是参考资料
- 重读文件比记住内容更高效
- 模型需要时可以重新读取

auto_compact 取末尾字符：
- 最新上下文最重要
- 保留最新最相关的对话
- 摘要保存完整历史
```

---

## 团队协作原则

### 何时需要团队

```
信号：
- 多个独立任务需要并行
- 任务需要不同专业知识
- 处理速度成为瓶颈
```

### 通信模式

```
Leader                    Teammate
   |                         |
   |--spawn----------------->|
   |                         |
   |<------work--------------|
   |                         |
   |--shutdown_request------>|
   |                         |
   |<-----shutdown_response--|
```

### 协议设计

所有协商遵循同一 FSM：

```
[pending] --approve--> [approved]
[pending] --reject---> [rejected]
```

**核心模式**：Request-Response + Request ID 关联。

### 自治程度

| 模式 | 特点 | 适用场景 |
|------|------|---------|
| 委托型 | 领导分配任务，队友执行 | 任务明确 |
| 自治型 | 队友自扫描、自认领 | 任务池模式 |

---

## 反模式与解决方案

| 反模式 | 问题 | 解决方案 |
|--------|------|---------|
| 过度工程 | 需求前上复杂度 | 从简单开始 |
| 太多工具 | 模型困惑 | 3-5 个起步 |
| 前置知识 | 上下文膨胀 | 按需加载 |
| 僵化工作流 | 无法适应 | 让模型决定 |
| 微观管理 | 削弱智能 | 信任模型 |

---

## 渐进复杂度层次

```
Level 0: Model + one tool
Level 1: Model + tool dispatch map
Level 2: Model + planning
Level 3: Model + subagents + skills
Level 4: Model + context management + persistence
Level 5: Model + teams + autonomy + isolation
```

**大多数 Agent 永远不需要超过 Level 2。**

---

## 附录 A：Agent Loop 最核心代码

这是唯一需要理解的**核心代码**（其余都是在此基础上的扩展）：

```python
def agent_loop(messages):
    while True:
        # 模型看到：对话历史 + 可用工具
        response = client.messages.create(
            model=MODEL,
            system=SYSTEM,
            messages=messages,
            tools=TOOLS,
        )
        messages.append({"role": "assistant", "content": response.content})

        # 模型决定：行动还是响应
        if response.stop_reason != "tool_use":
            return  # 响应用户，循环结束

        # 执行工具
        results = []
        for block in response.content:
            if block.type == "tool_use":
                output = TOOL_HANDLERS[block.name](**block.input)
                results.append({
                    "type": "tool_result",
                    "tool_use_id": block.id,
                    "content": output,
                })
        messages.append({"role": "user", "content": results})
        # 循环继续
```

**只有这一件事在循环里发生**：模型决定 → 执行 → 返回结果 → 继续。

---

## 附录 B：工具定义模板

```python
{
    "name": "tool_name",
    "description": "做什么 + 何时用",
    "input_schema": {
        "type": "object",
        "properties": {
            "param1": {
                "type": "string",
                "description": "参数含义"
            }
        },
        "required": ["param1"]
    }
}
```

---

## 附录 C：快速启动检查清单

当你拿到一个新业务，要构建 Agent 时：

```
□ 1. 明确 Agent 的领域和任务
□ 2. 列出 3-5 个核心工具
□ 3. 设计工具描述（何时用）
□ 4. 确定知识来源（哪些需要 Skill 文件）
□ 5. 选择规划机制（TodoWrite vs Task System）
□ 6. 决定是否需要团队（大多数不需要）
□ 7. 实现最小可行版本
□ 8. 观察模型失败模式
□ 9. 按需添加复杂性
```

---

## 附录 D：项目最新改进记录

### 1. micro_compact 保留 read_file 结果
read_file 输出是参考资料，重读比记忆更高效。

### 2. auto_compact 取末尾字符
取最后 80000 字符，保留最新最相关的上下文。

### 3. Task System 简化
只保留单向 `blockedBy`，去掉双向 `blocks`。

### 4. Nag Reminder 追加
reminder 追加到末尾，不打乱输出顺序。

### 5. 打印格式统一
长输出不再截断，更易调试。

---

## 核心启示

> **最好的 Agent 产品，出自那些明白自己的工作是 Harness 而非 Intelligence 的工程师之手。**

> **造好 Harness。Agent 会完成剩下的。**

> **Bash is all you need. Real agents are all the universe needs.**

---

**文档价值**：思想优先，代码为辅。
**使用方法**：理解原理，然后按需参考代码。
**分享时**： recipients 需要的是思维框架，不是代码模板。
