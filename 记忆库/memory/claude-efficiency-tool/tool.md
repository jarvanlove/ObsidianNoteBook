# Claude Code 效率工具完整指南

> 本文档整理了所有已安装的 Plugin、Skill、MCP 及其详细用法
> 生成时间: 2026-03-17
> 最后更新: 2026-03-17

---

## 目录

1. [Plugins (15个)](#一-plugins-15个)
2. [本地 Skills (8个)](#二-本地-skills-8个)
3. [MCP 工具](#三-mcp-工具)
4. [Hooks 配置](#四-hooks-配置)
5. [常用命令速查](#五-常用命令速查)
6. [OMC vs claude-code-spec-workflow 对比](#六-omc-vs-claude-code-spec-workflow-对比)

---

## 一、Plugins (15个)

### 1. superpowers@claude-plugins-official ⭐核心 (v5.0.4)

**作用**: Superpowers 是一个完整的软件开发工作流系统，基于一系列可组合的"技能"和初始指令构建。

**核心工作流**:

| 阶段 | 技能名 | 触发时机 | 说明 |
|------|--------|----------|------|
| 1 | brainstorming | 写代码前 | 通过提问细化想法，探索替代方案，分块展示设计 |
| 2 | using-git-worktrees | 设计批准后 | 在新分支创建隔离工作区，运行项目设置 |
| 3 | writing-plans | 有批准的设计 | 将工作分解为 2-5 分钟的小任务 |
| 4 | subagent-driven-development / executing-plans | 有计划时 | 为每个任务分派新子代理执行 |
| 5 | test-driven-development | 实现期间 | 强制 RED-GREEN-REFACTOR 循环 |
| 6 | requesting-code-review | 任务之间 | 对照计划审查，按严重程度报告问题 |
| 7 | finishing-a-development-branch | 任务完成时 | 验证测试，提供合并/PR/保留/丢弃选项 |

**子技能列表**:
- `brainstorming` - 创意头脑风暴
- `using-git-worktrees` - Git 工作树隔离
- `writing-plans` - 编写实施计划
- `subagent-driven-development` - 子代理驱动开发
- `executing-plans` - 执行计划
- `test-driven-development` - 测试驱动开发
- `requesting-code-review` - 请求代码审查
- `receiving-code-review` - 接收代码审查反馈
- `systematic-debugging` - 系统化调试
- `verification-before-completion` - 完成前验证
- `finishing-a-development-branch` - 完成开发分支
- `dispatching-parallel-agents` - 并行代理调度
- `using-superpowers` - 使用 superpowers

**GSD 命令**:
```bash
/gsd:new-project                      # 初始化新项目
/gsd:plan-phase                       # 创建阶段计划
/gsd:execute-phase                    # 执行阶段计划
/gsd:debug                            # 系统调试
/gsd:progress                         # 检查进度
/gsd:health                           # 诊断健康状态
/gsd:quick                            # 快速执行任务
```

**使用方法**:
- 技能会自动触发，无需手动调用
- 例如说"帮我计划这个功能"会自动触发 brainstorming

---

### 2. oh-my-claudecode@omc ⭐核心 (v4.8.0)

**作用**: OMC (oh-my-claudecode) 是多代理协调系统，为 Claude Code 提供零配置的团队协作能力。

**主要功能**:

#### Team Mode (推荐)
```bash
# 启动团队模式执行
/team 3:executor "fix all TypeScript errors"
```

执行流程: `team-plan → team-prd → team-exec → team-verify → team-fix (循环)`

#### tmux CLI Workers (v4.4.0+)
```bash
# 使用 Codex CLI
omc team 2:codex "review auth module for security issues"

# 使用 Gemini CLI
omc team 2:gemini "redesign UI components for accessibility"

# 使用 Claude CLI
omc team 1:claude "implement the payment flow"

# 查看状态
omc team status <task-name>

# 关闭任务
omc team shutdown <task-name>
```

#### 三模型综合 (/ccg)
```bash
# 同时询问 Codex + Gemini，Claude 综合结果
/ccg Review this PR — architecture (Codex) and UI components (Gemini)
```

#### 其他命令
```bash
# 深度访谈
/deep-interview "I want to build a task management app"

# 战略规划
/oh-my-claudecode:plan

# 自动执行
autopilot: build a REST API for managing tasks

# 更新 OMC
/oh-my-claudecode:update

# 诊断问题
/oh-my-claudecode:omc-doctor

# 设置 OMC
/oh-my-claudecode:omc-setup
```

---

### 3. claude-mem@thedotmack (v10.5.5)

**作用**: 跨会话持久化记忆系统，可搜索历史记录、创建分阶段实施计划。

**主要命令**:
```bash
# 搜索历史记忆
/claude-mem:mem-search "did we already solve this?"

# 创建实施计划
/claude-mem:make-plan "plan a feature"

# 执行计划
/claude-mem:do "execute the plan"

# 智能探索代码库
/claude-mem:smart-explore
```

---

### 4. cli-anything@cli-anything

**作用**: CLI Anything 工具，用于命令行操作增强。

**用法**: 通过自然语言执行 CLI 操作。

---

### 5. frontend-design@claude-plugins-official

**作用**: 生成高质量、非通用 AI 美学的前端界面。

**特点**:
- 大胆的美学选择
- 独特的字体和调色板
- 高冲击力动画和视觉细节
- 上下文感知实现

**使用方法**:
```bash
# 直接描述需求
"Create a dashboard for a music streaming app"
"Build a landing page for an AI security startup"
"Design a settings panel with dark mode"
```

---

### 6. context7@claude-plugins-official

**作用**: 查询最新技术文档和代码示例。

**支持的技术**: Next.js、Supabase、MongoDB、Express.js、React 等

**使用方法**:
```bash
# 解析库 ID
mcp__context7__resolve-library-id

# 查询文档
mcp__context7__query-docs
```

**示例**:
```bash
# 查询 Next.js 文档
"How to set up authentication with JWT in Express.js"
"React useEffect cleanup function examples"
```

---

### 7. code-review@claude-plugins-official

**作用**: 代码审查插件。

**使用方法**:
```bash
# 触发代码审查
/adversarial-review
```

---

### 8. github@claude-plugins-official

**作用**: GitHub MCP 集成，支持 PR、Issue、代码搜索等。

**可用工具**:
- `mcp__github__create_or_update_file` - 创建/更新文件
- `mcp__github__create_pull_request` - 创建 PR
- `mcp__github__create_issue` - 创建 Issue
- `mcp__github__get_file_contents` - 获取文件内容
- `mcp__github__list_commits` - 列出提交
- `mcp__github__list_issues` - 列出 Issues
- `mcp__github__list_pull_requests` - 列出 PR
- `mcp__github__search_code` - 搜索代码
- `mcp__github__search_issues` - 搜索 Issues
- `mcp__github__merge_pull_request` - 合并 PR

---

### 9. playwright@claude-plugins-official

**作用**: Playwright 浏览器自动化 MCP。

**可用工具**:
- `mcp__plugin_playwright_playwright__browser_navigate` - 导航到 URL
- `mcp__plugin_playwright_playwright__browser_click` - 点击元素
- `mcp__plugin_playwright_playwright__browser_type` - 输入文本
- `mcp__plugin_playwright_playwright__browser_snapshot` - 页面快照
- `mcp__plugin_playwright_playwright__browser_take_screenshot` - 截图
- `mcp__plugin_playwright_playwright__browser_evaluate` - 执行 JS
- `mcp__plugin_playwright_playwright__browser_console_messages` - 获取控制台消息

---

### 10. feature-dev@claude-plugins-official

**作用**: 功能开发引导技能，提供 7 阶段结构化工作流。

**7 阶段工作流**:
1. **Discovery** - 理解需要构建什么
2. **Codebase Exploration** - 探索相关现有代码
3. **Clarifying Questions** - 澄清问题，填补空白
4. **Architecture Design** - 架构设计
5. **Implementation Planning** - 实施计划
6. **Implementation** - 实施
7. **Quality Review** - 质量审查

**使用方法**:
```bash
/feature-dev Add user authentication with OAuth
```

---

### 11. ralph-loop@claude-plugins-official

**作用**: Ralph 自循环工作流，任务直到完成才停止。

**核心概念**: 基于持续 AI 代理循环，通过 Stop hook 拦截 Claude 的退出尝试，创建自引用反馈循环。

**使用方法**:
```bash
# 启动 Ralph 循环
/ralph-loop "Build a REST API for todos. Requirements: CRUD operations, input validation, tests. Output <promise>COMPLETE</promise> when done." --completion-promise "COMPLETE" --max-iterations 50

# 取消循环
/cancel-ralph
```

**参数**:
- `--max-iterations <n>` - N 次迭代后停止
- `--completion-promise <text>` - 标记完成的短语

---

### 12. code-simplifier@claude-plugins-official

**作用**: 代码简化优化技能，简化代码并提高可维护性。

**使用方法**:
```bash
/simplify
```

---

### 13. commit-commands@claude-plugins-official

**作用**: Git 提交相关快捷命令。

**命令**:

#### /commit
创建 git 提交，自动生成提交消息。

```bash
# 分析当前 git 状态，生成提交消息并提交
/commit
```

**特性**:
- 自动起草符合仓库风格的提交消息
- 遵循常规提交实践
- 避免提交包含敏感信息的文件

#### /commit-push-pr
完整工作流：提交、推送、创建 PR。

```bash
# 创建新分支（如果在 main 上）
# 暂存并提交更改
# 推送到 origin
# 使用 gh pr create 创建 PR
/commit-push-pr
```

**要求**:
- 必须安装并认证 GitHub CLI (`gh`)
- 仓库必须有名为 `origin` 的远程

#### /clean_gone
清理已从远程删除的本地分支。

```bash
# 列出所有本地分支
# 识别并删除 [gone] 分支
# 删除相关工作树
/clean_gone
```

---

### 14. figma@claude-plugins-official

**作用**: Figma 设计集成插件，支持从 Figma 获取设计稿并生成代码。

**功能**:
- 获取 Figma 文件内容
- 解析设计 token
- 生成对应前端代码

**使用方法**:
```bash
# 提供 Figma 文件 URL
"Convert this Figma design to React components: https://figma.com/file/..."
```

---

### 15. AI 研究技能 (来自 anthropic-agent-skills 和 ai-research-skills)

| 插件名 | 作用 |
|--------|------|
| document-skills | 文档处理相关技能集合 |
| example-skills | 示例技能集合 |
| tokenization | Tokenization 相关 |
| fine-tuning | 模型微调相关 |
| post-training | 训练后处理相关 |
| inference-serving | 推理服务相关 |
| distributed-training | 分布式训练相关 |
| optimization | 优化相关 |
| everything-claude-code | Everything Claude Code 综合工具集 |

---

## 二、本地 Skills (8个)

位于 `~/.claude/skills/` 或 `~/.agents/skills/`

### 1. adversarial-review

**作用**: 对抗性代码审查，使用对立模型（Claude ↔ Codex）进行多角度代码审查。

**使用方法**:
```bash
/adversarial-review
```

---

### 2. documentation-writer

**作用**: Diátaxis 文档框架专家，编写高质量技术文档。

**使用方法**: 自动触发或手动调用进行文档编写。

---

### 3. find-skills

**作用**: 帮助发现和安装可用的技能。

**使用方法**:
```bash
/find-skills
```

---

### 4. frontend-design (本地版本)

**作用**: 前端设计技能，创建精美界面。

---

### 5. humanizer

**作用**: 去除 AI 生成文本痕迹，使内容更自然、更像人类写作。

**基于**: Wikipedia 的"AI 写作迹象"指南

**检测和修复模式**:
- 过度象征主义
- 促销语言
- 表面的 -ing 分析
- 模糊归因
- em dash 过度使用
- 三重排比
- AI 词汇词
- 负面平行结构
- 过多连接短语

**使用方法**:
```bash
/humanizer
```

---

### 6. paper-reading

**作用**: 论文阅读和分析，支持 PDF 研究论文。

**使用方法**:
```bash
/paper-reading
```

---

### 7. planning-with-files

**作用**: 基于文件的规划系统（Manus 风格），用于多步骤任务管理。

**创建的文件**:
- `task_plan.md` - 任务计划
- `findings.md` - 发现记录
- `progress.md` - 进度跟踪

**使用方法**:
```bash
/planning-with-files
```

---

### 8. ui-ux-pro-max

**作用**: UI/UX Pro Max 设计智能，含 50+ 样式、97 调色板、57 字体配对。

**规则类别（按优先级）**:

| 优先级 | 类别 | 影响 | 领域 |
|--------|------|------|------|
| 1 | Accessibility | CRITICAL | ux |
| 2 | Touch & Interaction | CRITICAL | ux |
| 3 | Performance | HIGH | ux |
| 4 | Layout & Responsive | HIGH | ux |
| 5 | Typography & Color | MEDIUM | typography, color |
| 6 | Animation | MEDIUM | ux |
| 7 | Style Selection | MEDIUM | style, product |
| 8 | Charts & Data | LOW | chart |

**使用方法**:
```bash
# 生成设计系统
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "<product_type> <industry> <keywords>" --design-system -p "Project Name"

# 示例
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "beauty spa wellness service" --design-system -p "Serenity Spa"
```

---

## 三、MCP 工具

### GitHub MCP (来自 github@claude-plugins-official)

**功能**: 创建/更新文件、PR、Issue、仓库搜索、代码搜索等

**完整工具列表**:
- `mcp__github__create_or_update_file`
- `mcp__github__push_files`
- `mcp__github__search_repositories`
- `mcp__github__create_repository`
- `mcp__github__get_file_contents`
- `mcp__github__list_commits`
- `mcp__github__list_issues`
- `mcp__github__list_pull_requests`
- `mcp__github__get_issue`
- `mcp__github__get_pull_request`
- `mcp__github__create_pull_request`
- `mcp__github__create_issue`
- `mcp__github__update_issue`
- `mcp__github__add_issue_comment`
- `mcp__github__search_code`
- `mcp__github__search_issues`
- `mcp__github__search_users`
- `mcp__github__create_pull_request_review`
- `mcp__github__merge_pull_request`
- `mcp__github__get_pull_request_files`
- `mcp__github__get_pull_request_status`
- `mcp__github__update_pull_request_branch`
- `mcp__github__get_pull_request_comments`
- `mcp__github__get_pull_request_reviews`

### Playwright MCP (来自 playwright@claude-plugins-official)

**功能**: 浏览器自动化、截图、页面操作、测试等

**完整工具列表**:
- `mcp__plugin_playwright_playwright__browser_navigate`
- `mcp__plugin_playwright_playwright__browser_navigate_back`
- `mcp__plugin_playwright_playwright__browser_click`
- `mcp__plugin_playwright_playwright__browser_type`
- `mcp__plugin_playwright_playwright__browser_fill_form`
- `mcp__plugin_playwright_playwright__browser_select_option`
- `mcp__plugin_playwright_playwright__browser_press_key`
- `mcp__plugin_playwright_playwright__browser_drag`
- `mcp__plugin_playwright_playwright__browser_hover`
- `mcp__plugin_playwright_playwright__browser_wait_for`
- `mcp__plugin_playwright_playwright__browser_evaluate`
- `mcp__plugin_playwright_playwright__browser_run_code`
- `mcp__plugin_playwright_playwright__browser_file_upload`
- `mcp__plugin_playwright_playwright__browser_handle_dialog`
- `mcp__plugin_playwright_playwright__browser_snapshot`
- `mcp__plugin_playwright_playwright__browser_take_screenshot`
- `mcp__plugin_playwright_playwright__browser_console_messages`
- `mcp__plugin_playwright_playwright__browser_network_requests`
- `mcp__plugin_playwright_playwright__browser_tabs`
- `mcp__plugin_playwright_playwright__browser_resize`
- `mcp__plugin_playwright_playwright__browser_close`
- `mcp__plugin_playwright_playwright__browser_install`

### Context7 MCP (来自 context7@claude-plugins-official)

**功能**: 查询最新技术文档和代码示例

**工具**:
- `mcp__context7__resolve-library-id` - 解析库 ID
- `mcp__context7__query-docs` - 查询文档

**使用流程**:
1. 先调用 `resolve-library-id` 获取库 ID
2. 再调用 `query-docs` 查询具体文档

### claude-mem MCP (来自 claude-mem@thedotmack)

**功能**: 跨会话记忆搜索和智能代码探索

**工具**:
- `mcp__plugin_claude-mem_mcp-search__search` - 搜索记忆
- `mcp__plugin_claude-mem_mcp-search__timeline` - 获取时间线上下文
- `mcp__plugin_claude-mem_mcp-search__get_observations` - 获取详细观察
- `mcp__plugin_claude-mem_mcp-search__smart_search` - 智能代码搜索
- `mcp__plugin_claude-mem_mcp-search__smart_outline` - 代码结构大纲
- `mcp__plugin_claude-mem_mcp-search__smart_unfold` - 展开特定符号

---

## 四、Hooks 配置

当前 `settings.json` 中配置的 Hooks:

### 1. PostToolUse (Write|Edit 后)

**触发**: 执行 Write 或 Edit 工具后

**动作**:
1. 显示代码质量检查提示
2. 调用 `strict-code-reviewer` agent 进行代码审查

### 2. SessionEnd

**触发**: 会话结束时

**当前配置**:
```json
{
  "hooks": [
    {
      "type": "command",
      "command": "node \"C:/Users/jarvan_iv/.claude/scripts/global-collect.js\"",
      "async": false,
      "timeout": 120
    }
  ]
}
```

**说明**: 同步执行 global-collect.js 脚本，收集会话信息

### 3. SessionStart

**触发**: 会话开始时

**动作**:
1. 加载 `lessons.md` 全局经验教训文件
2. 显示加载提示

---

## 五、常用命令速查

### OMC 相关
```bash
/oh-my-claudecode:omc-setup          # 设置 OMC
/oh-my-claudecode:team               # 启动多代理团队
/oh-my-claudecode:plan                # 战略规划
/oh-my-claudecode:cancel              # 取消当前模式
/oh-my-claudecode:update               # 更新 OMC
/oh-my-claudecode:omc-doctor          # 诊断问题
/deep-interview "..."                 # 深度访谈
autopilot: "..."                      # 自动执行
```

### Team 模式
```bash
/team 3:executor "task"               # 启动 3 个执行器
/team 2:codex "task"                  # 启动 2 个 Codex
/team 2:gemini "task"                 # 启动 2 个 Gemini
/team 2:claude "task"                 # 启动 2 个 Claude
/ccg "task"                           # 三模型综合
```

### GSD 相关 (来自 superpowers)
```bash
/gsd:new-project                      # 初始化新项目
/gsd:plan-phase                       # 创建阶段计划
/gsd:execute-phase                    # 执行阶段计划
/gsd:debug                            # 系统调试
/gsd:progress                         # 检查进度
/gsd:health                           # 诊断健康状态
/gsd:quick                            # 快速执行
```

### 代码质量
```bash
/simplify                             # 简化代码
/adversarial-review                   # 对抗性审查
/feature-dev "..."                    # 功能开发
```

### Git 工作流
```bash
/commit                               # 提交代码
/commit-push-pr                       # 提交+推送+创建 PR
/clean_gone                           # 清理 gone 分支
```

### 设计
```bash
/frontend-design                      # 前端设计
/ui-ux-pro-max                        # UI/UX Pro Max
```

### 其他
```bash
/humanizer                            # 去除 AI 痕迹
/paper-reading                        # 阅读论文
/planning-with-files                  # 文件规划
/find-skills                          # 查找技能
/ralph-loop "..."                     # Ralph 循环
/cancel-ralph                         # 取消 Ralph
```

---

## 六、OMC vs claude-code-spec-workflow 对比

### 简介

| 项目 | **oh-my-claudecode (OMC)** | **claude-code-spec-workflow** |
|------|---------------------------|------------------------------|
| **作者** | Yeachan-Heo | Pimzino |
| **定位** | 多代理协调系统 | 规范驱动开发 (SDD) 工作流 |
| **核心理念** | Don't learn Claude Code. Just use OMC. | Spec-Driven Development |
| **安装方式** | Plugin + npm | npm global |
| **当前状态** | 活跃开发 (v4.8.0) | 转向 MCP 版本，CLI 版维护减少 |

---

### 核心工作流对比

#### OMC 工作流

```
用户请求 → OMC 调度器
                ↓
    ┌───────────┼───────────┐
    ↓           ↓           ↓
 deep-interview  plan        autopilot
    ↓           ↓           ↓
需求澄清    创建 PLAN.md   自动执行
    ↓           ↓           ↓
    └───────────┴───────────┘
                ↓
          执行 (子代理)
                ↓
          验证 + 修复循环
```

#### claude-code-spec-workflow 工作流

```
用户请求 → /spec-create
                ↓
    ┌───────────┼───────────┐
    ↓           ↓           ↓
Requirements  Design      Tasks
需求文档      设计文档    任务列表
    ↓           ↓           ↓
    └───────────┴───────────┘
                ↓
          /spec-execute
                ↓
          按任务 ID 执行
```

---

### 命令对比

| 场景 | OMC | claude-code-spec-workflow |
|------|-----|---------------------------|
| **初始化** | `/setup` 或 `/omc-setup` | `claude-code-spec-workflow` |
| **创建规范** | `/plan` 或 `autopilot:` | `/spec-create <name>` |
| **需求分析** | `/deep-interview` | `/spec-requirements` |
| **设计文档** | 自动包含在 plan 中 | `/spec-design` |
| **生成任务** | 自动分解 | `/spec-tasks` |
| **执行任务** | `/do` 或自动执行 | `/spec-execute <task-id>` |
| **查看状态** | 文件系统 + HUD | `/spec-status` |
| **Bug 修复** | `/debug` 或 `/ralph-loop` | `/bug-create` → `/bug-fix` |
| **列表查看** | 文件系统 | `/spec-list` |

---

### 项目结构对比

#### OMC 项目结构
```
.claude/
├── .omc/                      # OMC 状态目录
│   ├── state/
│   ├── notepad.md            # 记事本
│   ├── project-memory.json   # 项目记忆
│   └── plans/                # 计划文件
├── hud/omc-hud.mjs           # 状态栏
├── hooks/                     # 各种 hooks
└── scripts/                   # 辅助脚本
```

#### claude-code-spec-workflow 项目结构
```
.claude/
├── commands/                  # 14+ slash 命令定义
├── steering/                  # 指导性文档
│   ├── product.md            # 产品定义
│   ├── tech.md               # 技术栈
│   └── structure.md          # 项目结构
├── templates/                 # 文档模板
├── specs/                     # 生成的规范文件
├── bugs/                      # Bug 工作流
└── agents/                    # AI agents
```

---

### 核心特点对比

| 特性 | OMC | claude-code-spec-workflow |
|------|-----|---------------------------|
| **Steering Documents** | ❌ 无 | ✅ 有 (product.md, tech.md, structure.md) |
| **实时仪表板** | ✅ HUD | ✅ WebSocket Dashboard |
| **规范文档** | 自动生成 PLAN.md | 详细的 spec 文档 |
| **多模型支持** | ✅ Claude/Codex/Gemini | ❌ 仅 Claude |
| **团队模式** | ✅ `/team` | ❌ 无 |
| **自动迭代** | ✅ `/ralph-loop` | ❌ 无 |
| **深度访谈** | ✅ `/deep-interview` | ❌ 无 |
| **Token 优化** | 中等 | ✅ 60-80% 减少 |
| **Bug 专用工作流** | 通用调试 | ✅ 专门的 bug workflow |
| **记忆系统** | ✅ 跨会话记忆 | ❌ 基于文件 |
| **TDD 强制** | ✅ Superpowers 技能 | ❌ 可选 |

---

### 使用场景对比

#### 选择 OMC 如果：
- ✅ 需要**零配置**快速开始
- ✅ 想要**自动执行**功能 (`autopilot:`)
- ✅ 需要**深度访谈**澄清需求
- ✅ 希望有**Ralph 循环**自动迭代
- ✅ 可能需要**团队模式**扩展
- ✅ 想要**跨会话记忆**
- ✅ 需要**多模型协调** (Claude + Codex + Gemini)

#### 选择 claude-code-spec-workflow 如果：
- ✅ 喜欢**详细的规范文档**
- ✅ 需要**Steering Documents** 保持项目上下文
- ✅ 重视**Token 使用效率** (60-80% 减少)
- ✅ 想要**专门的 Bug 修复工作流**
- ✅ 需要**实时 WebSocket 仪表板**
- ✅ 喜欢**按任务 ID 精确执行**

---

### 典型使用示例

#### OMC 示例
```bash
# 1. 深度访谈澄清需求
/deep-interview "我想做一个支持多用户的任务管理系统"

# 2. 自动执行 (全自动)
autopilot: 实现多用户任务管理系统，使用 Vue 3 + Spring Boot

# 或者手动控制：
# 2. 创建计划
/plan

# 3. 执行计划
/do

# 4. 自动迭代直到完成
/ralph-loop "修复所有测试错误" --completion-promise "DONE"
```

#### claude-code-spec-workflow 示例
```bash
# 1. 初始化 (一次性)
claude-code-spec-workflow

# 2. 创建规范
/spec-create task-management-system

# 3. 生成需求 (可选)
/spec-requirements

# 4. 生成设计 (可选)
/spec-design

# 5. 生成任务列表
/spec-tasks

# 6. 按 ID 执行任务
/spec-execute 1
/spec-execute 2

# Bug 修复
/bug-create login-error
/bug-analyze
/bug-fix
/bug-verify
```

---

### 架构哲学对比

#### OMC: 代理协调型
```
"不要学习 Claude Code，只需使用 OMC"

重点：减少用户的认知负担，让系统决定如何最好地完成任务
特点：
- 自动触发技能
- 智能任务分解
- 自适应执行策略
- 跨会话学习
```

#### claude-code-spec-workflow: 规范驱动型
```
"规范驱动开发 (Spec-Driven Development)"

重点：通过详细的规范文档确保开发方向正确
特点：
- 明确的阶段划分
- 详细的文档生成
- 人主导，AI 辅助
- 精确的进度控制
```

---

### 总结建议

| 你的情况 | 推荐选择 |
|----------|----------|
| 快速原型开发 | **OMC** (`autopilot:`) |
| 大型项目开发 | **claude-code-spec-workflow** (规范明确) |
| 需求经常变化 | **OMC** (灵活适应) |
| 需要详细文档 | **claude-code-spec-workflow** |
| 一个人开发 | **OMC** (更简单) |
| Token 受限 | **claude-code-spec-workflow** (优化好) |
| 可能扩展团队 | **OMC** (有团队模式) |
| 需要精确控制 | **claude-code-spec-workflow** |

---

### 参考链接

- **OMC**: https://github.com/Yeachan-Heo/oh-my-claudecode
- **claude-code-spec-workflow**: https://github.com/Pimzino/claude-code-spec-workflow
- **spec-workflow-mcp** (新版): https://github.com/Pimzino/spec-workflow-mcp

---

## 附录: 配置文件位置

| 配置项 | 路径 |
|--------|------|
| 全局设置 | `~/.claude/settings.json` |
| 项目设置 | `<project>/.claude/settings.json` |
| 插件缓存 | `~/.claude/plugins/cache/` |
| 本地 Skills | `~/.claude/skills/` |
| Agents Skills | `~/.agents/skills/` |
| Hooks 脚本 | `~/.claude/hooks/` |
| 自定义脚本 | `~/.claude/scripts/` |
| 全局记忆 | `~/.claude/CLAUDE.md` |
| 项目记忆 | `~/.claude/projects/<path>/memory/MEMORY.md` |
| 状态栏脚本 | `~/.claude/hooks/statusline.sh` |

---

*文档生成时间: 2026-03-17*
*Superpowers 版本: v5.0.4*
*OMC 版本: v4.8.0*
*claude-mem 版本: v10.5.5*
