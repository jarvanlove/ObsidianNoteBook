# Personal-Assistant

**个人助手项目**

📁 路径：`C:/Work/note/CursorWorkSpace/SALL/ZOE_Personal_Assistant`

🔧 功能：
- stitch MCP配置

---

### 2026-03-25

**需求：** 我的stitch mcp配置好了 也重启了 为什么 执行 /mcp 还是没有？

- 实现：在用户主目录创建.claude/mcp.json配置文件，从全局配置复制stitch配置到项目级配置
- 技术：Claude MCP JSON配置

**需求：** claude 全局 setting.json 我已经配置了 skipDangerousModePermissionPrompt=true 启动后还是不停让我确认

- 实现：创建PowerShell别名配置文件claude-alias.ps1，设置claude函数自动添加--dangerously-skip-permissions参数
- 技术：PowerShell Profile

**需求：** 将快捷启动 c 或 cl 修改为 calude -d 相当于 claude --dangerously-skip-permissions

- 实现：在Microsoft.PowerShell_profile.ps1中添加函数，支持任意额外参数传递
- 技术：PowerShell函数


### 2026-03-25

**需求：** stitch你还是要帮我配置并连接，claude mcp add stitch --transport http --url "https://stitch.googleapis.com/mcp" --header "X-Goog-Api-Key: [REDACTED_API_KEY]"

- 实现：在项目级.claude/mcp.json和根目录.mcp.json中配置了stitch MCP，但重启Claude后仍无法识别
- 技术：Claude Code MCP

