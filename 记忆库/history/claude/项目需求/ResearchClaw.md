# ResearchClaw

**科研Agent系统**

📁 路径：`C:/Work/note/CursorWorkSpace/ResearchClaw`

🔧 功能：
- 对话界面
- 技能库
- 工具库
- 定时任务
- Webhooks通知

---

### 2026-04-01

**需求：** 1.需要推送到服务器的文件全路径 2.替换到服务器后��需要重启backend服务就可以了？命令是什么？ 3.将罗列出来的问题全部修复后再进行 /review → /qa → /ship流程 4.关于DAG执行按钮以及team按钮我希望不要放在左边侧边栏当作入口

- 实现：列出了所有修改的文件路径，解释了重启backend命令，进行了/qa测试，测试了DAG和Team页面入口
- 技术：Docker/Docker Compose/Vue3/FastAPI

**需求：** 根目录README.md中不要出现ScienceClaw，项目概述不要出现100%复刻，功能清单改为ResearchClaw自己的功能清单

- 实现：重写了README.md，将所有ScienceClaw引用替换为ResearchClaw自有功能描述
- 技术：Markdown文档

**需求：** 将Docker部署流程与文件同步经验总结成文档

- 实现：在Obsidian知识库创建了Docker部署流程与文件同步经验.md、Phase1-6功能实现总结.md等文档
- 技术：Obsidian笔记


### 2026-04-01

**需求：** 告诉我 Phase 2-6 具体是什么功能，我继续写代码

- 实现：创建了 subagent.py, task_dag.py, shutdown.py, team.py 等核心文件
- 技术：FastAPI, MongoDB, Redis, Python

**需求：** 我是不是可以直接将整个项目直接上传到服务器，然后 docker-compose restart backend websearch

- 实现：部署到 Windows 服务器 Docker 环境
- 技术：Docker, docker-compose

**需求：** 关于 DAG 执行按钮以及 team 按钮，我希望不要放在左边侧边栏当作入口

- 实现：更新了路由和页面，将 DAG/Team 作为独立页面
- 技术：Vue3, Vue Router

**需求：** 1.检查一下我们的任务是否全部完成 2.根目录README.md 中不要出现 ScienceClaw

- 实现：创建了侵权风险检查报告，修改了 README 移除 ScienceClaw 引用
- 技术：文档整理

**需求：** 1.目前该项目是复刻 scienceclaw 然后我们再此基础上增加了一些增强实现,所以需要你检查一下我们复刻后是否存在侵权行为 2.你还要整理一下我们新的项目的技术架构 3.需要将项目页面的整体主题改变

- 实现：创建了侵权风险检查报告、技术架构文档、产品架构文档，使用 ui-ux-pro-max 进行 UI 改版
- 技术：多文档输出 + UI 设计


### 2026-04-02

**需求：** 基于目前master创建一个dev分支，提交代码时候先提交到dev分支，不要合并到master

- 实现：从master创建dev分支，所有提交都推送到dev分支
- 技术：Git

**需求：** UI主题改版 - 替换emoji为Lucide图标，然后重新设计各页面为Deep Space Violet主题

- 实现：创建design-system.css和tailwind.config.js，将所有emoji替换为Lucide图标和SVG，依次重新设计HomePage、ChatPage、SkillsPage、TeamPage、LoginPage、MainLayout、DAGPage等页面的主题风格
- 技术：Vue, Tailwind CSS, Lucide Icons


### 2026-04-02

**需求：** 1. UI主题改版 (进行中: 替换 emoji → Lucide 图标) 这个UI改版不仅仅是简单的修改，而是大规模的，因为昨天 api 达到限制，座椅你现在需要继续昨天下午的工作 2.目前 gitlab 上只有一个 master 分支，我希望你帮我基于目前 master 创建一个 dev分支,提交代码时候先提交到dev分支，不要合并到 master 因为我们现在正在修改页面 可能出现很多问题 懂吗

- 实现：创建 dev 分支，提交 emoji 替换为 Lucide 图标的更改到 dev 分支
- 技术：Vue3 + Tailwind CSS + Lucide Icons

**需求：** 1.本地不需要进行环境搭建 2.我在服务器上已经部署好了，现在只是要修改UI整体风格和很多细节调整，因为这个是复刻别人的，不想和别人一样  2.你现在要做的就是先确定修改后的主题基调 然后所有页面依次修改（不想需要改动的就没必要改）3.等你改好一个版本后我会在服务器上重新部署 前端服务 然后再精细化让你修改

- 实现：确定 Deep Space Violet 主题基调，逐步修改各页面，保留好的交互设计
- 技术：Vue3 + Tailwind CSS + 自定义 design-system.css

**需求：** 直接使用 /frontend-design 技能对每个页面进行专业设计实现，因为设计系统基础已存在。例如：/frontend-design 重新设计 HomePage.vue，采用 Deep Research 风格 /frontend-design 重新设计 ChatPage.vue，采用 Modern Scientific 风格 这个是你刚刚建议的，希望可以融合 1 进行修改 3.我要的不是全部重改，别人的很多交互设计是很棒的，但是我需要你整体更换一下主题，万一别人看到 不会觉得这是我们的东西 能明白吗？  4.在修改UI这个工作中 需要你使用gstack 进行跟踪管理，实时告诉我进度

- 实现：使用 Deep Space Violet 主题统一重设计 HomePage、ChatPage、SkillsPage 等页面，使用 gstack 跟踪进度
- 技术：Vue3 + Tailwind CSS + Deep Space Violet 主题

**需求：** 1.确定这个方向 2.执行顺序 第一步：/ui-ux-pro-max - 选择风格（如：Scientific Premium、Biotech Modern、Academic Clean）- 生成 design-system.css 包含 CSS 变量- 输出 Tailwind 配置扩展 第二步：/frontend-design - 基于生成的设计系统- 为每个页面创建/更新样式- 确保组件一致性 当前已应用的基础 昨天已手动创建：- design-system.css - 基础设计系统- tailwind.config.js - 自定义颜色和动画- 所有 emoji 已替换为 Lucide 图标

- 实现：使用 /frontend-design 技能对各页面进行 Deep Space Violet 主题重设计
- 技术：Vue3 + Tailwind CSS + design-system.css + Lucide Icons

**需求：** 1.帮我总结当前项目都有哪些功能（精细程度需要达到按钮级别），哪些页面，这个项目是干什么的，用户怎使用这些功能（页面如何操作），这是一个很重大的工作，希望你可以认真对待，好好分析， 分析结果总结成经验

- 实现：分析 ResearchClaw 项目所有页面和功能，生成详细的功能与页面文档
- 技术：Vue3 + 前端分析


### 2026-04-02

**需求：** 目前gitlab上只有一个master分支，我希望你帮我基于目前master创建一个dev分支

- 实现：基于master创建dev分支并将代码推送到dev分支
- 技术：Git

**需求：** UI主题改版(替换emoji → Lucide图标)

- 实现：创建design-system.css和tailwind.config.js，将所有emoji替换为Lucide图标
- 技术：Vue3/Tailwind

**需求：** 现在需要继续昨天下午的工作，确定修改后的主题基调然后所有页面依次修改

- 实现：确定Linear浅色风格主题，逐个修改页面样式
- 技术：Vue3/Tailwind

**需求：** Linear这个我去看了人家官网还是可以的

- 实现：将UI主题从Deep Space Violet暗色调改为Linear浅色风格
- 技术：Vue3/Tailwind

**需求：** 帮我总结当前项目都有哪些功能（精细程度需要达到按钮级别）

- 实现：分析所有页面和组件，创建ResearchClaw功能与页面完整文档
- 技术：文档

**需求：** 告诉我/buddy这个命令是干什么的，写好详细的文档（符合obsidian双向链接）

- 实现：创建Claude-Code-Buddy伴侣系统文档在C:/Work/note/ObsidianNoteBook/记忆库/语义记忆/通用/
- 技术：Obsidian

**需求：** 1.帮我总结当前项目都有哪些功能，哪些页面，这个项目是干什么的，用户怎使用这些功能。2.接下来我会让你重新设计文档，该文档是标准的markdown文档，还需要包含图片

- 实现：创建ResearchClaw-UI设计文档，包含所有页面的截图占位符和结构说明
- 技术：Obsidian

**需求：** 1.那么你来帮我重新配置gitlab啊 2.刚刚更新的前端，Tools Library点击Finish这个工具失败了说没找到接口api/v1/tooluniverse/tools/Finish

- 实现：修复/backend/route/tooluniverse.py中添加Finish路由
- 技术：Python/FastAPI

**需求：** Team Coordination页面、Task DAG页面这新增的功能页面什么时候才会显示？入口在哪里？

- 实现：在ChatPage中添加DAGPanel和TeamPanel组件，由后端通过SSE协议触发显示
- 技术：Vue3/SSE

**需求：** 1.实现吧，不过不能用想当前的功能和页面，相反的，若出现这2个触发时机，你要和当前进行兼容（当前的交互行为）2.实现好了之后告诉我修改了哪些文件

- 实现：创建DAGPanel.vue和TeamPanel.vue，修改event.ts、sse_protocol.py，添加新路由
- 技术：Vue3/Python/SSE

