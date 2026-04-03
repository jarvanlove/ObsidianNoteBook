# 类Claw思想Agent快速开发技术选型

> 类型: 技术选型总结
> 更新: 2026-03-20
> 适用: 快速开发类OpenClaw/Manus思想的Agent产品

---

## 1. Core Agent框架对比

### 1.1 多Agent编排框架

| 框架 | 定位 | 推荐度 | 说明 |
|------|------|--------|------|
| **LangGraph** | 多Agent状态机 | ⭐⭐⭐ | 成熟、生态好、支持循环 |
| CrewAI | 多Agent编排 | ⭐⭐ | 概念简单、适合入门 |
| AutoGen | 多Agent对话 | ⭐⭐ | 微软出品、偏对话场景 |
| TaskWeaver | 任务驱动 | ⭐ | 微软出品、偏数据分析 |

**推荐**: LangGraph（我们的选择）

**原因**:
- 支持条件边、并行边、循环边（反思机制）
- 与LangChain生态无缝集成
- 支持流式输出
- 生产级稳定

### 1.2 单一Agent框架

| 框架 | 定位 | 推荐度 | 说明 |
|------|------|--------|------|
| LangChain Agents | 工具调用Agent | ⭐⭐⭐ | 成熟、工具集成丰富 |
| DeepAgent | DeepResearch类 | ⭐ | 适合简单场景 |
| OpenAI Agents SDK | OpenAI官方 | ⭐⭐ | 新出、Python优先 |

**注意**: DeepAgent不适合复杂多Agent协作场景

---

## 2. MCP (Model Context Protocol)

### 2.1 为什么必须用MCP

```
不用MCP:                    用MCP:
每个Agent自己封装工具  →   统一工具协议
工具难以复用           →   工具可共享
调用方式不统一          →   自然语言调用任意工具
```

### 2.2 MCP实现选择

| 实现 | 阶段 | 推荐度 | 说明 |
|------|------|--------|------|
| **fastmcp** | Phase 1 | ⭐⭐⭐ | 上手快、简化开发 |
| **官方MCP Python SDK** | Phase 2 | ⭐⭐⭐ | 完整实现、稳定 |

**快速开发建议**: fastmcp先跑演示，Phase2切换官方SDK

### 2.3 必须的MCP Server

| MCP Server | 封装内容 | 优先级 |
|------------|---------|--------|
| MCP-Search | SearXNG / 学术搜索API | P0 |
| MCP-Crawl | Crawl4AI / 网页爬取 | P0 |
| MCP-Platform | 业务平台API | P0 |
| MCP-Tool | ToolUniverse / 专业工具 | P1 |
| MCP-Code | AIOSandbox / 代码执行 | P1 |

---

## 3. 记忆系统

### 3.1 三层记忆必须

| 层级 | 技术 | 用途 |
|------|------|------|
| 对话历史 | **Redis** | 短期上下文 |
| 语义记忆 | **pgvector** | 长期向量检索 |
| 用户Profile | **PostgreSQL** | 结构化偏好 |

### 3.2 向量数据库选择

| 数据库 | 推荐度 | 说明 |
|--------|--------|------|
| **pgvector** | ⭐⭐⭐ | 复用现有PG、部署简单 |
| Qdrant | ⭐⭐ | 轻量、单独部署 |
| Milvus | ⭐⭐ | 生产级、规模大 |
| ChromaDB | ⭐ | 原型验证 |

**快速开发**: pgvector（我们已选型）

---

## 4. LLM选择（国产）

### 4.1 国产LLM API

| 模型 | 提供商 | 推荐度 | 适用场景 |
|------|--------|--------|---------|
| **Kimi** | 月之暗面 | ⭐⭐⭐ | 长上下文、推理强 |
| **Qwen** | 阿里通义 | ⭐⭐⭐ | 中文强、生态好 |
| **MiniMax** | MiniMax | ⭐⭐ | 性价比高 |
| 智谱GLM | 智谱 | ⭐⭐ | 学术场景 |

### 4.2 模型路由

```python
# 按任务类型路由
def route_llm(task_type: str, model: str = None):
    if model:  # 用户指定
        return get_model(model)

    if task_type == "reasoning":
        return kimi  # 强推理
    elif task_type == "chinese":
        return qwen  # 中文强
    else:
        return minimax  # 通用
```

---

## 5. 快速开发技术栈（Phase 1）

### 5.1 最小可行技术栈

```
┌─────────────────────────────────────────────────────┐
│                     Python 3.11+                      │
├─────────────────────────────────────────────────────┤
│  Agent框架:     LangGraph + LangChain               │
│  Web框架:       FastAPI                             │
│  工具协议:       fastmcp                            │
│  会话缓存:       Redis                              │
│  向量检索:       pgvector                           │
│  LLM:           Kimi/Qwen/MiniMax (按需路由)       │
└─────────────────────────────────────────────────────┘
```

---

## 6. 类Claw产品核心技术点

### 6.1 必须有的能力

| 能力 | 技术实现 | 重要性 |
|------|---------|--------|
| **任务分解** | LangGraph Supervisor | ⭐⭐⭐ 必须 |
| **工具调用** | MCP Protocol | ⭐⭐⭐ 必须 |
| **反思机制** | Critic Agent + 循环 | ⭐⭐⭐ 必须 |
| **记忆系统** | Redis + pgvector | ⭐⭐ 必须 |
| **流式输出** | SSE / WebSocket | ⭐⭐ 必须 |
| **结果确认** | 用户交互确认 | ⭐⭐⭐ 必须 |

### 6.2 Claude/OpenClaw的核心启发

```
Claude/Manus的核心:
1. Agent先规划再执行
2. 工具调用不是简单函数，是MCP协议
3. Critic评审 + 循环直到满意
4. 记忆是系统级设计，不是附加功能
5. 用户确认是必须的，不能静默执行

对应技术实现:
- LangGraph状态机: 规划-执行-评审-确认
- MCP: 标准化工具调用
- Critic Loop: 反思机制
- Memory System: 三层记忆
- Human-in-the-loop: 用户确认
```

---

## 7. 避坑指南

### 7.1 不要用的技术

| 技术 | 原因 |
|------|------|
| Dify (作为核心) | 可视化但缺乏灵活性，无法实现复杂Agent逻辑 |
| LangChain Agents (单一) | 不支持多Agent协作和状态管理 |
| 直接调用LLM | 缺乏工具调用、记忆、反思机制 |

### 7.2 常见错误

1. **过早优化**: Phase 1不要上pgvector，先用Redis缓存
2. **忽视MCP**: 不用MCP会导致工具调用混乱
3. **无反思机制**: 一次生成不可控，必须加Critic
4. **忘记用户确认**: Agent执行敏感操作必须用户确认

---

## 8. 参考产品技术栈

### ScienceClaw (参考)
```
LangChain DeepAgent + AIOSandbox + SearXNG + Crawl4AI + ToolUniverse
```

### Manus (对标)
```
多Agent协作 + MCP + Memory + Critic + Human-in-loop
```

### 我们的选型
```
LangGraph + LangChain + fastmcp + Redis + pgvector + Kimi/Qwen/MiniMax
```

---

*本文档用于快速选型参考，具体架构设计见《Research-Agent技术架构方案》*
