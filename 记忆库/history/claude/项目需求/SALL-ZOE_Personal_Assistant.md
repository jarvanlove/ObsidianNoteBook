# SALL-ZOE_Personal_Assistant

**个人助手项目**

📁 路径：`C:\Work\note\CursorWorkSpace\SALL\ZOE_Personal_Assistant`

🔧 功能：
- 配置skills插件启用
- Windows兼容hooks音效通知

---

### 2026-03-05

**需求：** 1.frontend-design ui-ux-pro-max find-skills 是要启用的

- 实现：在settings.local.json中配置enabledPlugins和enabledSkills
- 技术：Claude Code settings.json

**需求：** 1.刚刚设置的那个 hooks,让你修改为 windows适配版本的，现在 我在 C:\Users\jarvan_iv\.claude\sounds 目录下放进去了 3个音频 ，希望可以按照这个目录下语音进行通知，配置好了之后请测试一下

- 实现：创建PowerShell播放脚本并修改hooks配置为Windows兼容方式
- 技术：PowerShell + Claude hooks

**需求：** 1.planning with files 和 document-skills 你通过 find-skills 来进行安装吧 安装后请启用

- 实现：使用find-skills安装planning-with-files和documentation-writer
- 技术：Claude skills

