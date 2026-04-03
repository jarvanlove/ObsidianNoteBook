# Claude启动配置

**Claude Code启动优化**

📁 路径：`系统配置`

🔧 功能：
- 跳过权限确认
- 快捷命令

---

### 2026-03-25

**需求：** claude 全局 setting.json 我已经配置了 skipDangerousModePermissionPrompt: true  为什么我每次启动仍需要 --dangerously-skip-permissions

- 实现：用户提到已配置跳过权限确认，但Claude Code仍提示确认
- 技术：Claude Code

**需求：** 我不希望 输入 c 或 cl 希望输入 claude 然后就相当于执行 claude --dangerously-skip-permissions

- 实现：用户希望创建快捷启动命令claude = claude --dangerously-skip-permissions
- 技术：PowerShell别名配置

