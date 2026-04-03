# Research-Agent Day 1 开发决策记录

> 日期: 2026-03-21
> 项目: #Research-Agent
> 阶段: Phase 1 - 环境搭建 + 项目骨架

---

## 决策记录

### 1. 项目初始化

**决策**: 参照技术架构方案创建项目目录结构

**目录结构**:
```
Research-Agent/
├── app/
│   ├── __init__.py
│   ├── main.py              # FastAPI入口
│   ├── agents/              # Agent定义
│   │   └── __init__.py
│   ├── mcp_servers/         # MCP工具服务器
│   │   └── __init__.py
│   ├── graph/               # LangGraph定义
│   │   └── __init__.py
│   └── core/                # 核心配置
│       ├── __init__.py
│       ├── config.py        # 配置管理
│       ├── llm.py           # LLM路由
│       └── redis_client.py  # Redis客户端
├── tests/
│   └── __init__.py
├── requirements.txt
├── .env.example
└── README.md (待创建)
```

**理由**: 完全遵循技术架构方案v1.2的目录设计

---

### 2. 技术选型确认

**决策**: 所有依赖使用国产/开源技术栈

**确认的技术**:
- Web框架: FastAPI + uvicorn
- Agent框架: LangGraph + LangChain
- 工具协议: fastmcp (Phase1)
- 会话缓存: Redis (本地已有)
- LLM: Kimi / Qwen / MiniMax 按需路由
- 向量数据库: pgvector (Phase2启用)
- HTTP客户端: httpx

**环境决策**:
- ❌ 不使用 Docker
- ✅ 本地环境，所有数据库中间件本地已有
- ✅ 直接pip安装依赖

---

### 3. LLM路由策略

**决策**: 按任务类型自动路由模型

```python
task_type路由:
- reasoning → Kimi (强推理)
- chinese → Qwen (中文内容)
- code → Qwen (代码任务)
- general → MiniMax (通用任务)
```

**理由**: 国产模型各有优势，按场景分配实现最优效果

---

### 4. 配置管理

**决策**: 使用pydantic-settings管理配置

**核心配置项**:
- LLM API Keys (Kimi/Qwen/MiniMax)
- Redis连接 (localhost:6379)
- PostgreSQL连接 (localhost:5432, Phase2使用)
- SearXNG/Crawl4AI地址
- Java平台API地址

**理由**: 符合12-Factor原则，支持.env文件

---

### 5. 日志系统

**决策**: 使用loguru

**配置**:
- 控制台输出，颜色高亮
- 包含时间戳、级别、模块名、函数名、行号
- 可配置LOG_LEVEL

**理由**: 比标准logging更简洁美观

---

## 完成情况

### ✅ Day 1 已完成 (2026-03-21)

| 任务 | 状态 | 备注 |
|------|------|------|
| 项目目录结构 | ✅ | 符合架构设计 |
| requirements.txt | ✅ | 核心依赖已列 |
| .env.example | ✅ | 配置模板 |
| config.py | ✅ | pydantic-settings |
| llm.py | ✅ | 三模型路由 |
| redis_client.py | ✅ | 异步Redis客户端 |
| main.py | ✅ | FastAPI骨架 + CORS + 日志 |
| tests/__init__.py | ✅ | 测试目录 |
| 安装依赖 | ✅ | 核心依赖已安装 |
| .env创建 + MiniMax Key | ✅ | 已填入sk-cp-oXwbAe1... |
| FastAPI启动验证 | ✅ | 端口8001运行中 |
| Redis连接测试 | ✅ | 读写正常 |
| 健康检查接口 | ✅ | `{"status":"healthy","redis":"ok"}` |

### ⚠️ 问题记录

| 问题 | 状态 | 解决方案 |
|------|------|----------|
| pip读取中文路径GBK编码错误 | ✅ 已解决 | Python脚本逐个安装 |
| 端口8000被占用 | ✅ 已解决 | 改用8001端口 |
| SearXNG/crawl4ai包不存在 | ✅ 已解决 | 这些是独立服务，通过HTTP调用 |

---

## 服务状态

- FastAPI运行中: http://localhost:8001
- Redis: localhost:6379 ✅
- MiniMax API Key: 已配置

---

*记录人: Claude | 完成时间: 2026-03-21 22:20*

---

*记录人: Claude*

---

## Day 2 完成情况 (2026-03-21 晚)

### ✅ 已完成

| 任务 | 状态 | 备注 |
|------|------|------|
| ResearchState定义 | ✅ | 完整状态机结构 |
| Router Agent | ✅ | 任务类型识别和路由 |
| Literature Agent | ✅ | 文献综述生成 |
| Critic Agent | ✅ | 反思评审机制 |
| Research Graph | ✅ | LangGraph主图编译 |
| MCP-Search封装 | ✅ | SearXNG HTTP客户端 |
| MCP-Crawl封装 | ✅ | Crawl4AI HTTP客户端 |
| main.py集成 | ✅ | 流式输出 + 对话接口 |
| 依赖修复 | ✅ | langchain-openai已添加 |

### ⚠️ 问题记录

| 问题 | 状态 | 解决方案 |
|------|------|----------|
| LangChain导入路径变更 | ✅ 已解决 | langchain_core.prompts, langchain_openai |
| langchain-openai缺失 | ✅ 已解决 | pip install langchain-openai |

### 新增文件

```
app/
├── graph/
│   ├── research_state.py    # 状态定义
│   └── research_graph.py    # LangGraph主图
├── agents/
│   ├── supervisor.py        # 路由Agent
│   ├── literature.py        # 文献综述Agent
│   └── critic.py            # 反思Agent
└── mcp_servers/
    ├── search.py            # SearXNG客户端
    └── crawl.py             # Crawl4AI客户端
```

### 服务状态

- FastAPI: http://localhost:8001 ✅
- Redis: localhost:6379 ✅
- MiniMax API: 已配置

---

*记录人: Claude | Day 2完成: 2026-03-21 22:45*

---

## Day 3-9 完成情况 (2026-03-21 晚)

### ✅ 已完成

#### MCP-Platform实现
| 任务 | 状态 | 备注 |
|------|------|------|
| PlatformClient | ✅ | Java平台API封装 |
| 项目管理API | ✅ | list_projects, get_project, get_project_progress |
| 里程碑API | ✅ | list_milestones, create_milestone, get_milestone |
| 用户API | ✅ | get_current_user, get_user_projects |
| 仪表盘API | ✅ | get_dashboard_stats, get_dashboard_warnings |
| 便捷函数 | ✅ | sync_to_milestone, get_user_context |

#### Platform Agent
| 任务 | 状态 | 备注 |
|------|------|------|
| Platform Agent | ✅ | 平台操作节点 |
| 操作分类 | ✅ | sync/list/progress/dashboard |
| 用户上下文 | ✅ | 获取用户和项目信息 |

#### Memory Agent
| 任务 | 状态 | 备注 |
|------|------|------|
| Memory Agent | ✅ | 对话记忆保存/加载 |
| Redis存储 | ✅ | 7天过期 |

#### LangGraph更新
| 任务 | 状态 | 备注 |
|------|------|------|
| 完整Agent图 | ✅ | Router + Literature + Critic + Platform + Memory |
| 反思机制 | ✅ | 最多3轮循环 |
| 记忆保存 | ✅ | 完成后自动保存 |

#### Vue集成
| 任务 | 状态 | 备注 |
|------|------|------|
| TypeScript API客户端 | ✅ | researchAgentApi.ts |
| Vue组件 | ✅ | AgentChat.vue |
| 集成文档 | ✅ | README.md |

#### Java平台对接
| 任务 | 状态 | 备注 |
|------|------|------|
| 对接方案 | ✅ | MCP-Platform对接方案.md |
| API封装 | ✅ | HTTP调用Java REST API |

### 新增文件

```
app/
├── agents/
│   ├── platform.py           # 平台操作Agent
│   └── memory.py            # 记忆Agent
└── mcp_servers/
    └── platform.py           # Java平台API客户端

vue-integration/
├── researchAgentApi.ts      # TypeScript API客户端
├── AgentChat.vue           # Vue对话组件
└── README.md               # 集成文档

MCP-Platform对接方案.md      # Java对接方案文档
```

### API接口 (完整)
| 接口 | 方法 | 状态 |
|------|------|------|
| /health | GET | ✅ |
| /api/agent/chat | POST | ✅ |
| /api/agent/chat/stream | POST | ✅ |
| /api/agent/milestone/sync | POST | ✅ |
| /ws/agent/{session_id} | WebSocket | ✅ |

### 待确认 (Java端)
| 问题 | 优先级 | 状态 |
|------|--------|------|
| Java API Base URL | P0 | 待确认 |
| 认证方式 | P0 | 待确认 |
| CORS配置 | P1 | 待确认 |

---

## Day 10-12 完成情况 (2026-03-21 晚)

### Code Review 修复

| 问题 | 修复方案 | 状态 |
|------|---------|------|
| CORS过宽 | 限制来源列表 | ✅ |
| 无认证中间件 | app/middleware/auth.py | ✅ |
| WebSocket无认证 | 添加token参数警告 | ✅ |
| 异常信息暴露 | 不返回原始错误 | ✅ |
| 限流中间件 | app/middleware/rate_limit.py | ✅ |
| Critic JSON解析安全 | 严格验证+长度限制 | ✅ |
| asyncio未使用 | 移除未使用导入 | ✅ |
| httpx timeout处理 | 所有方法添加HTTPStatusError/TimeoutException | ✅ |
| upload_attachment token | 使用统一headers方法 | ✅ |

### 测试

| 测试类型 | 覆盖范围 | 状态 |
|---------|---------|------|
| 单元测试 | 16个测试用例 | ✅ 全部通过 |
| API测试 | /health, /chat, /milestone/sync | ✅ 通过 |

### 新增文件

```
app/middleware/
├── __init__.py
├── auth.py          # 认证中间件
└── rate_limit.py    # 限流中间件

tests/
└── test_agents.py   # 单元测试 (16个测试)
```

---

*记录人: Claude | Day 3-9完成: 2026-03-21 | Day 10-12完成: 2026-03-21*
