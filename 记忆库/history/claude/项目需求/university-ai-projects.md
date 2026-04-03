# university-ai-projects

**高校AI项目平台**

📁 路径：`/root/university-ai-projects`

🔧 功能：
- 首页搭建
- 红头文件链接解析
- 项目列表URL状态筛选

---

### 2026-03-23

**需求：** 告诉我他们的操作命令（实习生从toc分支拉取代码到自己的远程分支）

- 实现：提供git fetch origin && git merge origin/toc解决冲突后push的命令
- 技术：Git

**需求：** 先让2个实习生分别拉取toc分支的最新代码到自己的分支，出现冲突解决（以toc分支为准），然后push到自己分支，给出操作命令

- 实现：提供git fetch origin && git merge origin/toc --no-edit（冲突手动解决，以toc为准用theirs策略）后git push的命令
- 技术：Git

**需求：** 提交后需要将代码合并到toc分支并切换回toc-wjw

- 实现：执行git checkout toc && git merge toc-wjw && git push origin toc && git checkout toc-wjw
- 技术：Git

