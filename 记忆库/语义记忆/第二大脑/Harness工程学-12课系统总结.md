# #第二大脑 #Harness工程学

# Harness 工程学：12课系统总结

> 来源: C:\Work\note\CursorWorkSpace\learn-claude-code\docs\zh\
> 原始文档: README-zh.md + s01-s12 全部课程
> 吸收日期: 2026-03-21
> 理解深度: 基于 s01/s03/s04/s05/s07 原文深度分析

---

## 核心命题（必须刻入骨髓）

```
模型就是 Agent。代码是 Harness。造好 Harness，Agent 会完成剩下的。
```

**Agent = 训练出来的模型**（神经网络、Transformer、RNN）
**Harness = Tools + Knowledge + Observation + Action Interfaces + Permissions**

不是框架，不是提示词链，不是拖拽工作流。任何用 if-else/节点图/硬编码路由假装有智能的东西，都是 GOFAI 的现代还魂。

---

## Agent Loop（s01）— 一切的基础

```
User → messages[] → LLM → response
                          ↓
              stop_reason == "tool_use"?
             /                          \
          yes                           no
           |                             |
    execute tools                   return text
    append results
    loop back → messages[]
```

**Agent Loop 是唯一不变的东西**。后面 11 个课程全部是在这个循环上加机制。循环属于 agent，机制属于 harness。

**Python 实现（~30行）**：
```python
def agent_loop(query):
    messages = [{"role": "user", "content": query}]
    while True:
        response = client.messages.create(
            model=MODEL, system=SYSTEM,
            messages=messages, tools=TOOLS, max_tokens=8000,
        )
        messages.append({"role": "assistant", "content": response.content})
        if response.stop_reason != "tool_use":
            return
        results = []
        for block in response.content:
            if block.type == "tool_use":
                output = TOOL_HANDLERS[block.name](**block.input)
                results.append({"type": "tool_result", "tool_use_id": block.id, "content": output})
        messages.append({"role": "user", "content": results})
```

**一个退出条件控制一切**。`stop_reason != "tool_use"` 即停止。

---

## Tool Use（s02）— 给 Agent 一双手

**格言**: *"加一个工具，只加一个 handler"*

循环不动，新工具注册进 dispatch map 就行。

```python
TOOL_HANDLERS = {
    "bash": run_bash,
    "read": read_file,
    "write": write_file,
    # 新工具只需注册到这里
}
```

工具设计原则：**原子化、可组合、描述清晰**。

---

## TodoWrite（s03）— 让 Agent 不跑偏

**格言**: *"没有计划的 agent 走哪算哪"*

问题：多步任务中，模型会丢失进度，工具结果不断填上下文注意力被稀释。

**解决方案：带状态的 TodoManager + nag reminder**

```python
class TodoManager:
    def update(self, items: list) -> str:
        # 验证：同时只能有一个 in_progress
        validated, in_progress_count = [], 0
        for item in items:
            if item.get("status") == "in_progress":
                in_progress_count += 1
        if in_progress_count > 1:
            raise ValueError("Only one task can be in_progress")
        self.items = validated
        return self.render()
```

**nag reminder**: 连续 3 轮不调用 `todo`，自动注入 `<reminder>Update your todos.</reminder>`

**"同时只能有一个 in_progress"** → 强制顺序聚焦。

---

## Subagent（s04）— 大任务拆小

**格言**: *"大任务拆小，每个小任务干净的上下文"*

问题：工作越久 messages 越胖，父智能体被 5 个文件的读取结果污染，只需要一个词 "pytest"。

```
Parent: messages=[...长期累积...]  →  dispatch → Subagent: messages=[]（全新的）
                                                ↓
                                           tool calls...
                                                ↓
                                           return "pytest"
Parent context stays clean. Subagent context is discarded.
```

**关键**：子智能体以 `messages=[]` 启动，30+ 次工具调用后整个历史直接丢弃，只返回一段摘要文本。

**禁止递归**：子智能体的工具集中没有 `task` 工具，防止套娃。

---

## Skill Loading（s05）— 按需加载知识

**格言**: *"用到什么知识，临时加载什么知识"*

问题：10 个技能各 2000 token，全塞系统提示 = 20,000 token，大部分跟当前任务无关。

**两层架构**：
```
System prompt (Layer 1 — always present):
  Skills available:
    - git: Git workflow helpers        ~100 tokens/skill
    - test: Testing best practices

When model calls load_skill("git"):
  tool_result (Layer 2 — on demand):
    <skill name="git">
      Full git workflow instructions...  ~2000 tokens
    </skill>
```

**按需注入，不塞 system prompt**。模型知道有哪些技能（便宜），需要时再加载完整内容（贵）。

---

## Context Compression（s06）— 上下文总会满

**格言**: *"上下文总会满，要有办法腾地方"*

三层压缩策略换来无限会话。（s06 原文未细读，框架级理解）

---

## Task System（s07）— 持久化任务图

**格言**: *"大目标要拆成小任务，排好序，记在磁盘上"*

问题：s03 的 TodoManager 只是内存中的扁平清单，没有顺序、没有依赖、没有结构。

**解决方案：任务图（TaskManager）**

```
.tasks/
  task_1.json  {"id":1, "status":"completed"}
  task_2.json  {"id":2, "blockedBy":[1], "status":"pending"}
  task_3.json  {"id":3, "blockedBy":[1], "status":"pending"}
  task_4.json  {"id:4, "blockedBy":[2,3], "status":"pending"}
```

**三个核心问题**：
- **什么可以做？** → `pending` + `blockedBy` 为空
- **什么被卡住？** → `blockedBy` 不为空
- **什么做完了？** → `completed` → 自动解锁后续

**依赖边**：`blockedBy` + `blocks`。完成时自动清除依赖边。

**持久化**：磁盘 JSON 文件，压缩和重启后存活。s07 之后所有机制的协调骨架（s08 后台任务、s09+ 团队、s12 worktree 都读写这同一个结构）。

---

## 后台任务（s08）— 慢操作丢后台

**格言**: *"慢操作丢后台，agent 继续想下一步"*

守护线程 + 通知队列。后台跑命令，不阻塞主循环。

---

## Agent Teams（s09）— 一个人干不完的活分给队友

**格言**: *"任务太大一个人干不完，要能分给队友"*

持久化队友 + 异步 JSONL 邮箱。

---

## Team Protocols（s10）— 队友间统一沟通规矩

**格言**: *"队友之间要有统一的沟通规矩"*

request-response 模式驱动所有协商。关机 + 计划审批 FSM。

---

## 自治智能体（s11）— 队友自己看看板有活就认领

**格言**: *"队友自己看看板，有活就认领"*

不需要领导逐个分配，空闲轮询 + 自动认领，自组织。

---

## Worktree Isolation（s12）— 各干各的目录互不干扰

**格言**: *"各干各的目录，互不干扰"*

任务管目标，worktree 管目录，按 ID 绑定。隔离执行通道。

---

## Harness vs 第二大脑系统对照

| Harness 组件 | learn-claude-code 实现 | 第二大脑对应 |
|-------------|----------------------|-------------|
| Tools | bash/read/write/dispatch map | Claude Code 内置工具 |
| Knowledge | skills/\*/SKILL.md 按需加载 | 语义记忆（情景记忆+语义记忆） |
| Observation | git diff/错误日志/浏览器状态 | 读取任务清单、记忆文件 |
| Action Interfaces | CLI / API / UI 交互 | 写入记忆、提交 Git |
| Permissions | 沙箱/审批/信任边界 | FATAL 规则 |
| Task System | .tasks/task_*.json 任务图 | 任务清单.md |
| Subagent | run_subagent() 干净上下文 | 暂无（未来扩展） |
| Background Tasks | 守护线程 | 暂无（未来扩展） |

**第二大脑 ≈ 第二层（Harness 的 Knowledge 层）+ 第四层（Task 持久化层）**

---

## 元信息

- **文档作者**: shareAI-lab
- **GitHub**: https://github.com/shareAI-lab/learn-claude-code
- **产品**: Kode Agent CLI (`npm i -g @shareai-lab/kode`)、Kode Agent SDK、Claw0（姐妹项目）
- **标签**: #Harness工程学 #Agent理论 #12课 #ClaudeCode #KodeAgent
