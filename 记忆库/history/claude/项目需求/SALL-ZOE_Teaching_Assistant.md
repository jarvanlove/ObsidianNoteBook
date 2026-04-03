# SALL-ZOE_Teaching_Assistant

**助教插件项目**

📁 路径：`C:\Work\note\CursorWorkSpace\SALL\ZOE_Teaching_Assistant`

🔧 功能：
- Chrome插件登录功能
- 学校教案内置模板功能

---

### 2026-03-05

**需求：** 将插件安装在同时浏览器上，点击登录，后出现网络错误

- 实现：检查popup.js中API_BASE配置和登录请求逻辑
- 技术：Chrome Extension + Backend API

**需求：** 1.拿到了学校教案的真实表单所有字段，如下，所以我觉我们之前设计的那个模板表可以用上了，将这个表单做成内置的模板，页面不需要变动，然后大文本或富文本里面用户选中的内容会有很多存在级联，但是修饰属于改节点下（也就是该表单key下的值，在对比分析自动填充时候要注意细节处理）

- 实现：创建builtinTemplate.ts，修改templateService.ts支持内置模板，修改前端模板选择逻辑
- 技术：Node.js + Prisma + Vue

**需求：** templates 中 is_builtin 字段什么意思？我说的内置模板，是因为目前只有这一个模板，所以你只需要按照当前 templates 表结构进行初始化即可

- 实现：创建seed-templates.sql脚本按照当前表结构初始化内置模板
- 技术：PostgreSQL + Prisma

