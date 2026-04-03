# ObsidianNoteBook（第二大脑系统）

**基于 Claude Code + Obsidian 的跨平台第二大脑记忆系统**

📁 路径：`C:\Work\note\ObsidianNoteBook`

🔧 功能：
- 三层记忆架构（情景记忆/语义记忆/强制规则）
- v2.0 自动项目路由
- 任务追踪与时间意图捕获
- 外部知识吸收机制
- Git 版本控制

---

### 2026-03-21

**需求：** 启动第二大脑

- 实现：创建了完整的第二大脑系统，包含记忆库目录结构、强制规则、使用指南等
- 技术：Claude Code + Obsidian + Git

**需求：** 我想把这个东西做成可执行的一个工具，用户可以通过命令进行安装、卸载、升级等

- 实现：开发成 npm 包 @jiawenwu/secondbrain，包含 init/upgrade/uninstall/status/doctor 命令，支持 /secondbrain slash command
- 技术：Node.js + npm + Claude Code CLI

**需求：** 命令有点少啊！现在只有 初始化 安装 卸载 升级 具体怎么用 不是也要命令码？

- 实现：slash command 直接显示使用说明，包含常用指令表（启动第二大脑、记录决策、总结经验等）
- 技术：Claude Code Skill

**需求：** 我希望也能这样（像 gstack 一样通过 /gstack 使用）

- 实现：创建 SKILL.md 并部署到 ~/.claude/skills/secondbrain/，支持 /secondbrain 或 /sb 命令
- 技术：Claude Code Skill System

