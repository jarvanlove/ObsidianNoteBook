# 第二大脑
#第二大脑

## 架构说明（v2.0）

### 双层架构

| 层级 | 组件 | 触发方式 | 作用 |
|------|------|---------|------|
| 第一层 | CLAUDE.md 全局配置 | 启动时自动加载（无声） | 路由规则 + 强制规则 + 记忆路径 |
| 第二层 | /secondbrain Skill | 用户手动激活 | 读取任务清单 + 显示今日概览 |

### 工作流程

```
Claude Code 启动
    ↓
加载 CLAUDE.md → 路由表、项目指针就绪（无声）
    ↓
用户输入 /secondbrain
    ↓
Skill 读取 任务清单.md → 显示今日概览
```

### 项目自动识别

根据工作目录自动识别当前项目：

| 工作目录 | 项目名 | 语义记忆路径 |
|---------|--------|------------|
| `C:\Work\note\CursorWorkSpace\SALL-ZOE\Link-AI-Sci-Z-Platform-College` | #科研智能体 | 语义记忆/科研智能体/ |
| `C:\Work\note\CursorWorkSpace\SALL-ZOE\Link-AI-Sci-Z-Platform-Backend` | #科研智能体后端 | 语义记忆/科研智能体/ |
| `C:\Work\note\CursorWorkSpace\SALL-ZOE\Personal-Assistant` | #SALL-ZOE-个人助手 | 语义记忆/SALL-ZOE-个人助手/ |
| `C:\Work\note\ObsidianNoteBook` | #第二大脑 | 语义记忆/第二大脑/ |
| `C:\Work\note\FreelanceWork` | #FreelanceWork | 语义记忆/FreelanceWork/ |

### 查询命令

| 你说的话 | 效果 |
|---------|------|
| `启动第二大脑` | 读任务清单，显示今日概览 |
| `查看#某项目的记忆` | 筛选该项目所有情景+语义记忆 |
| `记录这个决策` | 写入情景记忆，自动打当前项目标签 |
| `总结成经验` | 写入语义记忆，自动打当前项目标签 |
| `这是通用知识` | 写入语义记忆/通用/（无项目标签） |

### FATAL 规则

- FATAL-001: 禁止擅自重组文件结构
- FATAL-002: 删除用户内容而非归档
- FATAL-003: 忽略 Obsidian 链接（每个 wikilink 都是资产）
- FATAL-004: 跳过结构性变更的审批流程
- FATAL-005: Git 操作未经授权（禁止自动 commit）
