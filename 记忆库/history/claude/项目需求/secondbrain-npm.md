# secondbrain-npm

**第二大脑CLI工具npm包**

📁 路径：`C:\Work\note\ObsidianNoteBook`

🔧 功能：
- 命令行工具sb/secondbrain
- 自动初始化记忆库
- SKILL.md自包含路由
- 环境变量自定义路径

---

### 2026-03-21

**需求：** 将第二大脑做成可执行npm工具，用户可通过命令安装、卸载、升级

- 实现：创建@jiawenwu/secondbrain包，实现init/upgrade/uninstall/status/doctor命令，设置/sb快捷命令
- 技术：Node.js + npm

**需求：** 命令名改为sb，记忆库默认路径为C:\Work\note\ObsidianNoteBook\记忆库

- 实现：修改package.json bin配置，支持SECONDBRAIN_PATH环境变量覆盖默认路径
- 技术：Node.js

**需求：** npm包需要开源，不能包含用户本地路径和项目文档

- 实现：创建templates目录存放模板文件，从现有记忆库提取并清理用户数据
- 技术：Node.js + npm


### 2026-03-21

**需求：** 将第二大脑做成可执行npm工具

- 实现：创建@jiawenwu/secondbrain npm包，包含bin/sb.js入口和src模块
- 技术：Node.js CLI

**需求：** 通过/sb使用

- 实现：修改slash命令为/secondbrain
- 技术：技能配置

**需求：** 用户安装后能否获取项目内容

- 实现：需要在sb init时配置CLAUDE.md实现全局加载
- 技术：配置集成


### 2026-03-21

**需求：** 把第二大脑做成可执行的npm工具

- 实现：创建了NPM包@jiawenwu/secondbrain，包含init/upgrade/uninstall/status/doctor命令和/secondbrain斜杠命令
- 技术：Node.js CLI


### 2026-03-21

**需求：** 我想把这个东西做成可执行的一个工具，用户可以通过命令进行安装、卸载、升级等

- 实现：创建npm包@jiawenwu/secondbrain，包含sb init/upgrade/uninstall/status/doctor命令
- 技术：Node.js npm

**需求：** 命令有点长，像gstack那样用/sb或/secondbrain

- 实现：实现/secondbrain斜杠命令
- 技术：Claude Code slash command

**需求：** 用户安装后执行sb init后打开claude code执行/secondbrain能不能获取到对应项目的内容

- 实现：在init.js中追加第二大脑引用到CLAUDE.md，使SKILL.md自包含路由配置
- 技术：SKILL.md + CLAUDE.md集成

**需求：** 用户通过命令安装是不会配置CLAUDE.md的

- 实现：sb init时自动追加第二大脑配置到全局CLAUDE.md
- 技术：Node.js文件操作

