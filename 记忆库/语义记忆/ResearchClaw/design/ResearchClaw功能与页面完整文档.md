# ResearchClaw 功能与页面完整文档

> 项目: #ResearchClaw
> 创建时间: 2026-04-02
> 精细程度: 按钮级别

---

## 一、项目概述

### 1.1 项目定位

**ResearchClaw（科研爪）** 是面向科研工作者的 AI 智能体平台，提供完整的 AI Agent 能力，帮助研究人员完成文献检索、数据分析、代码执行、任务自动化等科研工作。

### 1.2 核心价值

| 核心能力 | 说明 |
|---------|------|
| **对话式 AI** | SSE 流式输出，支持多轮对话和长期会话 |
| **Web 搜索** | 多源并行搜索（SearXNG 元搜索） |
| **网页爬取** | Crawl4AI 智能内容提取 |
| **文件处理** | PDF、DOCX、XLSX、PPTX 等多格式支持 |
| **代码执行** | Docker 沙箱安全执行 Python/Shell |
| **Skills 系统** | 内置+外部 Skills 扩展机制 |
| **ToolUniverse** | 1900+ 科研工具集成 |
| **任务调度** | Celery 异步任务 + 定时任务 |
| **Team Coordination** | 多 Agent 团队协作 |

### 1.3 系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend (Vue3)                      │
│                     localhost:5174                          │
└─────────────────────────────┬───────────────────────────────┘
                              │
┌─────────────────────────────▼───────────────────────────────┐
│                      Backend (FastAPI)                      │
│                     localhost:12001                         │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              DeepAgent Engine                        │  │
│  │  • Agent Loop  • Tools  • Skills  • Middleware      │  │
│  └──────────────────────────────────────────────────────┘  │
└──────┬──────────────┬──────────────┬──────────────┬─────────┘
       │              │              │              │
┌──────▼────┐  ┌──────▼────┐  ┌──────▼────┐  ┌──────▼─────┐
│  MongoDB  │  │   Redis   │  │  SearXNG  │  │  Sandbox   │
│  :27014   │  │   :6379   │  │   :26080   │  │   :18080   │
└───────────┘  └───────────┘  └───────────┘  └────────────┘
```

---

## 二、页面与功能详解（按钮级别）

### 2.1 页面总览

| 页面 | 路由 | 功能定位 |
|------|------|---------|
| **LoginPage** | `/login` | 登录/注册/密码重置 |
| **HomePage** | `/chat` | 首页欢迎 + 快速入口 |
| **ChatPage** | `/chat/{session_id}` | 主对话界面 |
| **MainLayout** | - | 主布局框架（含左侧导航） |
| **SkillsPage** | `/chat/skills` | Skills 技能库管理 |
| **SkillDetailPage** | `/chat/skills/{skillName}` | Skill 文件浏览 |
| **ToolsPage** | `/chat/tools` | 工具库（1900+科研工具） |
| **ToolDetailPage** | `/chat/tools/{toolId}` | 工具详情页 |
| **ScienceToolDetail** | `/chat/science-tools/{toolId}` | 科学工具详情 |
| **TasksPage** | `/chat/tasks` | 定时任务列表 |
| **TaskConfigPage** | `/chat/tasks/{id}/edit` 或 `/chat/tasks/new` | 任务配置 |
| **TasksListPage** | `/chat/tasks` | 任务列表（卡片视图） |
| **TeamPage** | `/chat/team` | 团队协作管理 |
| **DAGPage** | `/chat/dag` | 任务流程图可视化 |
| **SharePage** | `/share/{session_id}` | 分享会话查看 |
| **ShareLayout** | - | 分享页布局 |

---

## 三、详细功能分解

### 3.1 登录模块（LoginPage）

**路由**: `/login`

#### 表单元素

| 元素 | 类型 | 说明 |
|------|------|------|
| 邮箱/用户名输入框 | Input | 登录凭证 |
| 密码输入框 | Input | 含 Show/Hide 切换按钮 |
| 忘记密码链接 | Link | 跳转密码重置 |

#### 按钮

| 按钮名称 | 位置 | 功能 |
|---------|------|------|
| **Login** | 主表单 | 提交登录表单（蓝紫渐变按钮） |
| **Show/Hide** | 密码字段 | 切换密码显示/隐藏 |
| **Register** | 表单底部链接 | 跳转注册页面 |

#### 操作流程

1. 用户输入邮箱/用户名 + 密码
2. 点击 **Login** 按钮提交
3. 系统验证成功后跳转首页

---

### 3.2 首页（HomePage）

**路由**: `/chat`

#### 页面结构

```
┌─────────────────────────────────────────────────────────────┐
│  Logo + RobotAvatar                    用户菜单 (Avatar)     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Welcome Area                                               │
│  ├── 问候语（打字机效果）                                    │
│  └── 副标题循环（科研问题/蛋白质问题/药物数据/科学洞察）      │
│                                                             │
│  ┌─────────────┐ ┌─────────────┐                           │
│  │ Quick Card 1 │ │ Quick Card 2 │                          │
│  │ 蛋白质与疾病 │ │ 药物安全档案 │                          │
│  └─────────────┘ └─────────────┘                           │
│  ┌─────────────┐ ┌─────────────┐                           │
│  │ Quick Card 3 │ │ Quick Card 4 │                          │
│  │ 化合物ADMET │ │ 文献与数据  │                           │
│  └─────────────┘ └─────────────┘                           │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ ChatBox                                             │    │
│  │ [输入框]                    [模型选择] [发送]         │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Quick Prompt Cards（4个快速开始卡片）

| 卡片 | 图标 | 描述 | 操作 |
|------|------|------|------|
| **蛋白质与疾病** | 分子图标 | Multi-step target analysis pipeline | Hover 显示 Try it 箭头 |
| **药物安全档案** | 药物图标 | FDA adverse event analysis with data visualization | Hover 显示 Try it 箭头 |
| **化合物 ADMET** | 化学图标 | Multi-dimensional drug property prediction | Hover 显示 Try it 箭头 |
| **文献与数据** | 文献图标 | Cross-database research synthesis | Hover 显示 Try it 箭头 |

#### ChatBox 组件

| 元素 | 类型 | 说明 |
|------|------|------|
| 输入框 | Textarea | 输入行数可调 |
| 附件按钮 | Paperclip 图标 | 上传文件 |
| 模型选择器 | Dropdown | 选择 AI 模型 |
| 发送按钮 | Button | 提交对话 |

#### 操作流程

1. 用户可直接在 ChatBox 输入问题
2. 或点击 Quick Card 填充预设问题
3. 选择模型后点击发送

---

### 3.3 主对话页面（ChatPage）

**路由**: `/chat/{session_id}`

#### 页面布局

```
┌─────────────────────────────────────────────────────────────┐
│                      Top Bar                               │
│  [← 返回]  会话标题                         [分享] [文件]  │
├──────────┬──────────────────────────────────┬───────────────┤
│          │                                  │               │
│ LeftPanel│         Message Area             │ ActivityPanel│
│          │                                  │               │
│ [导航]   │  [用户消息]                       │ [思考过程]   │
│ [会话列表]│  [AI消息 + 工具调用]              │ [执行时间线] │
│          │  [ProcessMessage]                │               │
│          │                                  │               │
├──────────┴──────────────────────────────────┴───────────────┤
│                      ChatBox                               │
│  [附件] [输入框...              ] [机器人] [模型] [✨] [发送]│
└─────────────────────────────────────────────────────────────┘
```

#### Top Bar 按钮

| 按钮 | 图标 | 功能 |
|------|------|------|
| **返回** | ArrowLeft | 返回首页 |
| **分享** | Share2 | 打开分享弹窗 |
| **文件** | Folder | 显示会话文件列表 |

#### 分享弹窗（Share Popover）

| 选项 | 图标 | 说明 |
|------|------|------|
| **Private Only** | Lock | 仅自己可见 |
| **Public Access** | Globe | 链接可见 |
| **Share Instantly** | Button | 分享（私有模式） |
| **Copy Link** | Button | 复制链接（公开模式） |

#### ChatBox 组件

| 元素 | 按钮/图标 | 功能 |
|------|----------|------|
| 附件 | Paperclip | 上传文件 |
| 机器人头像 | RobotAvatar | 下拉菜单（ResearchClaw/Skills 管理） |
| 模型选择 | Dropdown | 选择对话模型 |
| 优化 | MagicWand | Prompt 优化 |
| 发送 | Send Arrow | 发送消息 |

#### Skills 下拉菜单

| 元素 | 说明 |
|------|------|
| ResearchClaw 默认助理 | 内置默认技能 |
| Skills 列表 | 用户安装的技能 |
| **Eye/EyeOff** | 阻止/取消阻止技能 |
| **Trash2** | 删除技能 |

#### 消息区域

| 元素 | 说明 |
|------|------|
| Process indicator | 可点击展开 Activity Panel |
| Tool count badge | 显示调用工具数量 |
| Reasoning/Running/Failed | 状态徽章 |

#### Activity Panel（右侧）

- 显示思考过程
- 显示执行时间线
- 点击工具打开 ToolPanel

#### 操作流程

1. 用户在 ChatBox 输入问题
2. 可上传附件（PDF/DOCX 等）
3. 可选择技能/模型
4. 点击发送，AI 流式响应
5. 可查看工具调用详情
6. 可分享会话

---

### 3.4 左侧导航面板（LeftPanel）

**位置**: ChatPage 左侧抽屉

#### 导航标签页

| 图标 | 名称 | 路由 |
|------|------|------|
| MessageSquare | Chat | `/chat` |
| Blocks | Skills | `/chat/skills` |
| Wrench | Tools | `/chat/tools` |
| CalendarClock | Tasks | `/chat/tasks` |
| Settings2 | Settings | 设置弹窗 |
| User | 用户菜单 | - |

#### 会话列表

| 功能 | 说明 |
|------|------|
| 搜索框 | 搜索会话 |
| 过滤标签 | All / Pinned / Running（含计数） |
| **+ New Task** | 新建任务（渐变按钮） |
| 会话分组 | Today / Yesterday / Previous 7 Days / Older |
| 会话项 | 显示标题 + 预览 |

#### 操作流程

1. 点击导航图标切换页面
2. 在搜索框搜索会话
3. 点击会话进入对话
4. 点击 + New Task 创建新任务会话

---

### 3.5 Skills 技能库（SkillsPage）

**路由**: `/chat/skills`

#### 页面结构

```
┌─────────────────────────────────────────────────────────────┐
│  Skills Library (数量)                        [搜索框]      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │ Skill Card │ │ Skill Card │ │ Skill Card  │           │
│  │ [头像]      │ │ [头像]      │ │ [头像]      │           │
│  │ 名称        │ │ 名称        │ │ 名称        │           │
│  │ 文件数      │ │ 文件数      │ │ 文件数      │           │
│  │ [阻止] [删除]│ │ [阻止] [删除]│ │ [阻止] [删除]│           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
│                                                             │
│  空状态: "No external skills installed"                    │
│  提示: "Use the chat to install skills via find-skills"   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Skill Card 按钮

| 按钮 | 图标 | 功能 |
|------|------|------|
| **Block** | EyeOff | 阻止/取消阻止技能 |
| **Delete** | Trash2 | 删除技能（需确认对话框） |

#### 删除确认对话框

| 按钮 | 说明 |
|------|------|
| **Cancel** | 取消删除 |
| **Delete** | 确认删除（红色按钮） |

#### Skill 详情页（SkillDetailPage）

**路由**: `/chat/skills/{skillName}`

| 元素 | 说明 |
|------|------|
| 返回按钮 | ArrowLeft |
| 文件树侧边栏 | 浏览目录结构 |
| 文件夹导航 | Up / 路径面包屑 |
| 文件预览区 | 查看文件内容 |
| 文件类型 | 文本/二进制区分 |

#### 操作流程

1. 在 SkillsPage 查看所有技能
2. 点击技能卡片的阻止/删除按钮
3. 点击技能名称进入详情页
4. 在详情页浏览文件树和内容

---

### 3.6 Tools 工具库（ToolsPage）

**路由**: `/chat/tools`

#### 标签页

| 标签 | 说明 |
|------|------|
| **Science** | 1900+ 科学工具 |
| **External** | 用户安装的外部工具 |

#### Science Tools 界面

```
┌──────────────┬──────────────────────────────────────────────┐
│ Categories   │  Tools Grid                                 │
│              │                                              │
│ [All Tools]  │  ┌────────┐ ┌────────┐ ┌────────┐          │
│  - 蛋白质    │  │ Tool   │ │ Tool   │ │ Tool   │          │
│  - 药物      │  │ Card   │ │ Card   │ │ Card   │          │
│  - 文献      │  │        │ │        │ │        │          │
│  - ...       │  │ [Open] │ │ [Open] │ │ [Open] │          │
│              │  └────────┘ └────────┘ └────────┘          │
│              │                                              │
│              │  [Show more]                                │
└──────────────┴──────────────────────────────────────────────┘
```

#### Tool Card 信息

| 元素 | 说明 |
|------|------|
| 名称 | 工具名称 |
| 描述 | 一句话说明 |
| 参数数量 | 参数个数徽章 |
| Has examples | 是否有示例 |
| **Open** | 打开详情链接 |

#### External Tools 界面

| 元素 | 说明 |
|------|------|
| 名称 + 文件 | 工具标识 |
| 描述 | 工具说明 |
| **Eye/EyeOff** | 阻止/取消阻止 |
| **Trash2** | 删除工具 |

#### 删除确认

| 按钮 | 说明 |
|------|------|
| **Cancel** | 取消 |
| **Delete** | 确认删除 |

#### 操作流程

1. 切换 Science/External 标签
2. 在侧边栏选择分类
3. 浏览工具卡片
4. 点击 Open 查看详情
5. 可阻止/删除外部工具

---

### 3.7 Tasks 任务管理（T历)

**路由**: `/chat/tasks`

#### TasksListPage（任务列表卡片视图）

**页面顶部**

| 元素 | 说明 |
|------|------|
| 页面标题 | "Scheduled Tasks" |
| 任务计数 | 显示任务总数 |
| **+ New Scheduled Task** | 新建任务按钮（渐变蓝绿色） |

**任务卡片**

| 元素 | 位置 | 说明 |
|------|------|------|
| 任务名称 | 卡片头部 | 标题文本 |
| 状态徽章 | Running/Disabled | 绿/灰色徽章 |
| **编辑按钮** | 右上角 | Pencil 图标，hover 显示 |
| 时钟图标 + 时间 | 信息区 | 下次运行时间 |
| 网络钩子图标 | 信息区 | Webhook 配置状态 |
| 日历图标 | 信息区 | 调度描述 |
| 活动图标 | 信息区 | 运行次数 + 成功率 |

**卡片操作按钮**

| 按钮 | 条件 | 功能 |
|------|------|------|
| **View details** | 所有任务 | 打开运行历史对话框 |
| **Disable/Enable** | 根据状态 | 切换启用/禁用 |
| **Delete** | 所有任务 | 删除任务（需确认） |

**运行历史对话框**

| 元素 | 说明 |
|------|------|
| 表头 | 任务名 + 总执行次数 + 成功率 |
| 每页条数选择 | Select: 10/20/50/100 |
| 历史表格 | 时间 / 状态 / 结果 |
| **View** | 查看运行详情 |
| **Copy link** | 复制会话链接 |
| 分页 | Previous / Next |

**查看结果弹窗**

| 元素 | 说明 |
|------|------|
| DialogTitle | "Result" |
| 内容区 | 预格式化文本显示结果/错误 |

#### TaskConfigPage（任务配置页）

**路由**: `/chat/tasks/new` 或 `/chat/tasks/{id}/edit`

**表单字段**

| 字段 | 类型 | 说明 |
|------|------|------|
| Task name | Input | 任务名称 |
| Model | Dropdown | 选择模型（ProviderIcon + 名称） |
| Schedule | Input + Button | 调度时间描述 |
| Prompt input | Textarea | 发送给模型的提示词 |
| Webhook | Multi-select | 通知网络钩子 |
| Status | Select（仅编辑） | Enabled / Disabled |

**Schedule 配置详情**

| 元素 | 说明 |
|------|------|
| 输入框 | 只读，点击打开选择面板 |
| **Verify** | 验证调度表达式 |
| 推荐调度 | 预设选项列表 |
| 自定义输入 | 手动输入调度描述 |

**Schedule 反馈状态**

| 状态 | 显示内容 |
|------|---------|
| 验证中 | "Verifying..." |
| 验证失败 | 红色错误提示 + AlertCircle |
| 验证成功 | 绿色成功 + 下次运行时间 |

**Webhook 配置**

| 元素 | 说明 |
|------|------|
| 已选 Webhook 列表 | 类型徽章 + 名称 + X 移除 |
| **+ Add webhook** | 添加按钮（虚线边框） |
| Webhook 下拉 | 可用 Webhook 列表 |
| **Manage webhooks in Settings** | 跳转设置 |

**表单按钮**

| 按钮 | 说明 |
|------|------|
| **Save / Create** | 提交表单（渐变蓝绿色） |
| **Cancel** | 取消并返回列表 |

#### 操作流程

1. 在 TasksPage 查看所有任务
2. 点击 **+ New Scheduled Task** 创建新任务
3. 填写任务名称、选择模型
4. 配置调度时间（可用推荐或自定义）
5. 编写 Prompt
6. 配置 Webhook 通知
7. 点击 **Create/Save**
8. 可在任务卡片上 Enable/Disable/Delete

---

### 3.8 Team 团队协作（TeamPage）

**路由**: `/chat/team`

#### 页面结构

```
┌─────────────────────────────────────────────────────────────┐
│  [← 返回]                              [+ 添加成员]          │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────┐ ┌──────────┐ ┌──────────┐                    │
│  │ Total    │ │ Idle     │ │ Busy     │                    │
│  │ Members  │ │ Count    │ │ Count    │                    │
│  └──────────┘ └──────────┘ └──────────┘                    │
│                                                             │
│  Member Cards                                               │
│  ┌─────────────┐ ┌─────────────┐                          │
│  │ Avatar      │ │ Avatar      │                          │
│  │ Name/Role   │ │ Name/Role   │                          │
│  │ Status      │ │ Status      │                          │
│  │ Current Task│ │ Current Task│                          │
│  │ [Assign] [Remove] │ [Assign] [Remove] │                  │
│  └─────────────┘ └─────────────┘                          │
│                                                             │
│  Messages Section                                           │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ 消息列表                                             │   │
│  │ [广播输入框]  [发送]                                 │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Header 按钮

| 按钮 | 说明 |
|------|------|
| **← 返回** | 返回上一页 |
| **+ 添加成员** | 打开添加成员弹窗 |

#### Member Card 信息

| 元素 | 说明 |
|------|------|
| Avatar | 首字母圆形头像 |
| Name | 成员名称 |
| Role | 角色（researcher/coder/reviewer/general） |
| Status | Idle / Busy 徽章 |
| Current task | 当前执行的任务 |

#### Member Card 按钮

| 按钮 | 条件 | 功能 |
|------|------|------|
| **Assign Task** | 成员 Idle 时 | 打开分配任务弹窗 |
| **Remove** | 所有成员 | 移除成员 |

#### 添加成员弹窗

| 字段 | 类型 |
|------|------|
| Name | Input |
| Role | Select（researcher/coder/reviewer/general） |
| Capabilities | Input（多行文本） |

| 按钮 | 说明 |
|------|------|
| **Cancel** | 取消 |
| **Add** | 添加成员 |

#### 分配任务弹窗

| 字段 | 类型 |
|------|------|
| Task description | Textarea |

| 按钮 | 说明 |
|------|------|
| **Cancel** | 取消 |
| **Assign** | 分配任务 |

#### Messages 区域

| 元素 | 说明 |
|------|------|
| 消息列表 | 显示 from/type/content/time |
| 广播输入 + 发送 | 广播消息给所有成员 |

#### 操作流程

1. 查看团队成员概览
2. 点击 **+ 添加成员** 打开弹窗
3. 填写成员信息并添加
4. 为 Idle 成员分配任务
5. 可移除不需要的成员
6. 使用广播功能发送消息

---

### 3.9 DAG 任务流程图（DAGPage）

**路由**: `/chat/dag`

#### 页面结构

```
┌─────────────────────────────────────────────────────────────┐
│  [← 返回]                              [+ Add Task] [Execute]│
├─────────────────────────────────────────────────────────────┤
│  Stats Cards                                                │
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐  │
│  │ Total  │ │ Done ✓ │ │Running │ │Pending │ │Failed ✗│  │
│  │  5     │ │   2    │ │   1    │ │   1    │ │   1    │  │
│  └────────┘ └────────┘ └────────┘ └────────┘ └────────┘  │
│                                                             │
│  DAG Canvas (SVG)                                           │
│  ┌─────────────────────────────────────────────────────┐   │
│  │    [Task A] ───→ [Task B] ───→ [Task C]            │   │
│  │         ↓                                           │   │
│  │    [Task D] ───→ [Task E]                          │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  Task Detail Panel (点击节点打开)                           │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Task ID / Status / Depth / Blocked By / Result     │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### Header 按钮

| 按钮 | 说明 |
|------|------|
| **← 返回** | 返回上一页 |
| **+ Add Task** | 添加新任务 |
| **Execute DAG** | 执行整个 DAG |

#### Stats Cards

| 卡片 | 颜色 |
|------|------|
| **Total** | 默认 |
| **Completed** | 绿色 |
| **In progress** | 青色 |
| **Pending** | 灰色 |
| **Failed** | 红色 |

#### DAG Canvas

| 元素 | 说明 |
|------|------|
| 节点 | 任务名 + 状态 |
| 边 | 带箭头连接线 |
| 点击节点 | 打开详情面板 |

#### Add Task 弹窗

| 字段 | 类型 |
|------|------|
| Task ID | Input |
| Task name | Input |
| Description | Textarea |
| blocked_by | Multi-select（依赖任务） |

| 按钮 | 说明 |
|------|------|
| **Cancel** | 取消 |
| **Add** | 添加任务 |

#### Task Detail Panel

| 字段 | 说明 |
|------|------|
| Task ID | 任务标识 |
| Status | 状态徽章 |
| Depth | 依赖深度 |
| Blocked by | 依赖列表 |
| Result | 执行结果（完成后） |
| Error | 错误信息（失败时） |

#### 操作流程

1. 点击 **+ Add Task** 创建任务节点
2. 设置任务依赖关系（blocked_by）
3. 点击 **Execute DAG** 执行
4. 点击节点查看执行详情
5. 监控任务状态变化

---

### 3.10 Share 分享页面（SharePage）

**路由**: `/share/{session_id}`

#### 功能

| 元素 | 说明 |
|------|------|
| 分享内容 | 只读会话消息 |
| 重放控制 | Replay 按钮 |
| 返回 | 返回上一页 |

#### 操作流程

1. 通过分享链接访问
2. 查看分享的会话内容
3. 可重放 AI 思考过程

---

### 3.11 Settings 设置对话框

**触发方式**: LeftPanel 中的 Settings2 图标

#### 设置标签页

| 标签 | 说明 |
|------|------|
| **Account** | 账户设置 |
| **General** | 通用设置 |
| **Models** | 模型配置 |
| **Notifications** | 通知/Webhook 设置 |
| **Tasks** | 任务设置 |
| **Profile** | 个人资料 |

#### Notifications 设置

| 元素 | 说明 |
|------|------|
| Webhook 列表 | 已配置的 Webhook |
| **+ Add Webhook** | 添加通知渠道 |
| 类型选择 | 飞书/钉钉/企微 |
| 测试按钮 | 测试通知 |

#### 操作流程

1. 点击 Settings2 打开设置
2. 切换不同标签页
3. 配置相关选项
4. 保存设置

---

## 四、API 路由总览

### 4.1 认证模块（Auth）

| 方法 | 路径 | 功能 |
|------|------|------|
| GET | `/api/v1/auth/check-default-password` | 检查默认密码 |
| POST | `/api/v1/auth/login` | 登录 |
| POST | `/api/v1/auth/register` | 注册 |
| POST | `/api/v1/auth/logout` | 登出 |
| GET | `/api/v1/auth/status` | 认证状态 |
| POST | `/api/v1/auth/change-password` | 修改密码 |
| POST | `/api/v1/auth/refresh-token` | 刷新 Token |

### 4.2 会话模块（Sessions）

| 方法 | 路径 | 功能 |
|------|------|------|
| PUT | `/api/v1/sessions` | 创建会话 |
| GET | `/api/v1/sessions` | 列出会话 |
| GET | `/api/v1/sessions/shared/{session_id}` | 获取分享会话 |
| GET | `/api/v1/sessions/{session_id}` | 获取会话详情 |
| DELETE | `/api/v1/sessions/{session_id}` | 删除会话 |
| POST | `/api/v1/sessions/{session_id}/chat` | SSE 对话 |
| POST | `/api/v1/sessions/{session_id}/stop` | 停止会话 |
| POST | `/api/v1/sessions/{session_id}/share` | 分享会话 |
| DELETE | `/api/v1/sessions/{session_id}/share` | 取消分享 |
| GET | `/api/v1/sessions/{session_id}/files` | 沙箱文件列表 |
| GET | `/api/v1/sessions/{session_id}/sandbox-file` | 读取沙箱文件 |

### 4.3 任务模块（DAG）

| 方法 | 路径 | 功能 |
|------|------|------|
| POST | `/api/v1/dag` | 创建 DAG |
| GET | `/api/v1/dag/{dag_id}` | 获取 DAG 状态 |
| PUT | `/api/v1/dag/{dag_id}/task` | 添加任务 |
| GET | `/api/v1/dag/{dag_id}/status` | 获取执行状态 |
| GET | `/api/v1/dag/{dag_id}/task/{task_id}` | 获取任务状态 |
| DELETE | `/api/v1/dag/{dag_id}/task/{task_id}` | 删除任务 |
| DELETE | `/api/v1/dag/{dag_id}` | 删除 DAG |
| POST | `/api/v1/dag/{dag_id}/execute` | 执行 DAG |

### 4.4 团队模块（Team）

| 方法 | 路径 | 功能 |
|------|------|------|
| POST | `/api/v1/team` | 创建团队 |
| GET | `/api/v1/team/{team_id}/status` | 获取团队状态 |
| POST | `/api/v1/team/{team_id}/member` | 添加成员 |
| DELETE | `/api/v1/team/{team_id}/member/{member_id}` | 移除成员 |
| POST | `/api/v1/team/{team_id}/message` | 发送消息 |
| POST | `/api/v1/team/{team_id}/broadcast` | 广播消息 |
| POST | `/api/v1/team/{team_id}/shutdown` | 关闭团队 |

---

## 五、用户核心操作流程

### 5.1 日常对话流程

```
1. 打开 HomePage
   ↓
2. 在 ChatBox 输入问题
   或点击 Quick Prompt Card
   ↓
3. 可选：上传附件（PDF/DOCX 等）
   ↓
4. 可选：选择模型/Skill
   ↓
5. 点击发送
   ↓
6. 查看流式响应
   ↓
7. 可点击查看工具调用详情
   ↓
8. 对话自动保存在会话列表
```

### 5.2 创建定时任务流程

```
1. 进入 TasksPage
   ↓
2. 点击 + New Scheduled Task
   ↓
3. 填写任务名称
   ↓
4. 选择执行模型
   ↓
5. 配置调度时间
   - 选择推荐调度 或
   - 自定义输入（如 "Every day at 9am"）
   - 点击 Verify 验证
   ↓
6. 编写 Prompt
   ↓
7. 配置 Webhook（可选）
   ↓
8. 点击 Create
   ↓
9. 在任务列表查看/管理
```

### 5.3 团队协作流程

```
1. 进入 TeamPage
   ↓
2. 点击 + 添加成员
   ↓
3. 填写成员信息
   - Name
   - Role (researcher/coder/reviewer/general)
   - Capabilities
   ↓
4. 点击 Add
   ↓
5. 为 Idle 成员分配任务
   ↓
6. 成员执行任务
   ↓
7. 可通过广播发送消息
```

### 5.4 DAG 任务流编排流程

```
1. 进入 DAGPage
   ↓
2. 点击 + Add Task
   ↓
3. 填写任务信息
   - Task ID
   - Task name
   - Description
   - blocked_by（依赖任务）
   ↓
4. 点击 Add 添加到画布
   ↓
5. 重复添加多个任务
   ↓
6. 点击 Execute DAG
   ↓
7. 监控执行状态
   ↓
8. 点击节点查看详情
```

### 5.5 分享会话流程

```
1. 在 ChatPage 打开会话
   ↓
2. 点击分享按钮（Top Bar）
   ↓
3. 选择分享模式
   - Private Only（仅自己）
   - Public Access（任何人可看）
   ↓
4. Private: 点击 Share Instantly
   Public: 点击 Copy Link
   ↓
5. 将链接发送给其他人
   ↓
6. 接收者通过 SharePage 查看
```

---

## 六、技术栈总结

| 层级 | 技术 | 版本 |
|------|------|------|
| 前端框架 | Vue3 | 3.4+ |
| 状态管理 | Pinia | 2.1+ |
| 构建工具 | Vite | 5.0+ |
| 后端框架 | FastAPI | 0.110+ |
| Agent 引擎 | DeepAgents | - |
| 数据库 | MongoDB | 7.0 |
| 缓存 | Redis | 7.0 |
| 任务队列 | Celery | 5.3+ |
| 沙箱 | Docker | 24+ |
| LLM | DeepSeek | API v1 |

---

## 七、最近开发动态

### 7.1 UI 主题改版（Deep Space Violet）

最近的开发工作集中在 UI 主题改版：

| 提交 | 描述 |
|------|------|
| `dd71a1b` | ChatPage 重新设计 |
| `13ddca5` | HomePage 重新设计 |
| `ef4cd85` | SkillsPage 重新设计 |
| `60f9fdb` | TeamPage 重新设计 |
| `1b5d202` | MainLayout 和 LoginPage 重新设计 |
| `26fae4f` | DAGPage 重新设计 |
| `1285624` | LeftPanel 和 SessionItem 重新设计 |

### 7.2 图标替换

- 大量 emoji 图标替换为 Lucide SVG 图标
- 提升界面的专业性和一致性

---

## 八、文件结构

```
ResearchClaw/
├── ResearchClaw/
│   ├── backend/
│   │   ├── deepagent/          # 核心 Agent 引擎
│   │   ├── route/              # API 路由
│   │   │   ├── auth.py         # 认证
│   │   │   ├── chat.py         # 对话
│   │   │   ├── sessions.py     # 会话管理
│   │   │   ├── dag.py          # DAG
│   │   │   ├── team.py         # 团队
│   │   │   ├── file.py         # 文件操作
│   │   │   ├── memory.py       # 记忆
│   │   │   ├── models.py       # 模型配置
│   │   │   ├── science.py      # 科学工具
│   │   │   ├── statistics.py   # 统计
│   │   │   ├── task_settings.py# 任务设置
│   │   │   ├── tooluniverse.py # 工具宇宙
│   │   │   └── subagent.py     # 子 Agent
│   │   └── builtin_skills/     # 内置 Skills
│   │
│   └── frontend/
│       └── src/
│           ├── pages/           # 页面组件
│           ├── components/     # 公共组件
│           ├── api/             # API 调用
│           ├── stores/          # Pinia 状态
│           └── utils/           # 工具函数
│
├── Skills/                     # 外部 Skills
├── Tools/                      # 外部 Tools
└── workspace/                  # 工作目录
```

---

> 文档版本: 1.0
> 最后更新: 2026-04-02
> 维护建议: 随着功能迭代，定期更新此文档
