# MCP配置

**MCP服务配置**

📁 路径：`全局配置`

🔧 功能：
- stitch MCP集成
- GitHub MCP集成

---

### 2026-03-25

**需求：** 1. .mcp.json 文件是你帮我配置的，现在我使用 /mcp 命令没有显示呢? 3.stitch 你还是需要帮我配置并连接，claude mcp add stitch --transport http --url https://stitch.googleapis.com/mcp --header X-Goog-Api-Key:[REDACTED_API_KEY]

- 实现：创建了~/.claude/mcp.json配置文件，配置stitch MCP服务
- 技术：Claude Code MCP

