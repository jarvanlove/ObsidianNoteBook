# task-role-assignment

**根据任务描述自动分析、匹配并推荐最适合的Agency-Agents角色的Skill，内置176个角色**

📁 路径：`~/.claude/skills/task-role-assignment`

🔧 功能：
- 动态角色匹配
- 任务分析
- 激活提示词生成
- 推荐工具建议

---

### 2026-03-18

**需求：** 你要是做skill 难道不是应该将资源整合到一起 打包好 按照 skill 的标准去做吗？

- 实现：将.claude/agents/目录下的角色文档复制到skill的data目录下，修改matcher.py路径引用，更新SKILL.md描述
- 技术：Python + Claude Code Skill

**需求：** data 目录下能否将 .claude/agents/ 目录下所有角色文档全部挪到 data 目录下？后续我自己维护这些角色？

- 实现：将176个角色文档复制到data目录，修改matcher.py、README.md、SKILL.md中的路径引用
- 技术：Python + Claude Code Skill

**需求：** 而且这个 README.md能否和 SKILL.md 进行融合？

- 实现：融合README.md和SKILL.md的内容
- 技术：Markdown

**需求：** 不是一共有174个角色吗？然后你将未挪过来的角色 先挪到 .claude/agent/ 下 然后还需要同步 .claude\skills\task-role-assignment\data 目录下

- 实现：从agency-agents-zh项目读取最新角色列表，将缺失的16个角色（blender、godot、roblox、unity、unreal、reddit相关）复制到.claude/agents/和skill的data目录，最终同步到176个角色
- 技术：Git + Claude Code Skill

