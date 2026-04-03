# ResearchClaw Docker 部署流程与文件同步经验

> 项目: #ResearchClaw  
> 日期: 2026-04-01  
> 类型: 部署流程 + 问题排查  
> 关联: GitLab CI/CD, Docker Compose, Windows 服务器

---

## 一、GitLab 提交历史

本次迭代共 **9 个提交**：

| 提交哈希 | 提交信息 | 类型 |
|---------|---------|------|
| c901098 | fix(websearch): gracefully handle missing playwright module | Bug修复 |
| eb271cf | docs: rewrite README to remove ScienceClaw references | 文档 |
| 3f34c15 | docs: fix frontend port from 5173 to 5174 in README | 文档修正 |
| 6890754 | fix(frontend): import highlight.js via npm instead of CDN | Bug修复 |
| e7ee56d | chore(release): add VERSION and CHANGELOG for v0.1.0 | 版本管理 |
| 9dc1200 | fix(api): add global HTTPException handler for consistent JSON error format | Bug修复 |
| e86cbf6 | docs: reorganize documentation into docs/ directory | 文档整理 |
| 94a9d9b | fix(phase-2-4): review fixes - resource limits, error handling, signal safety | Bug修复 |
| 4cd080f | Initial commit: ResearchClaw base + Phase 1-6 enhancements | 初始提交 |

**GitLab 仓库**: http://192.168.1.241:9980/root/researchclaw  
**当前版本**: v0.1.0

---

## 二、修改文件详细信息

### 2.1 新增文件 (A)

| 文件路径 | 说明 | 服务器部署方式 |
|---------|------|---------------|
| `VERSION` | 版本号 0.1.0 | 直接复制 |
| `CHANGELOG.md` | 变更日志 | 直接复制 |
| `docs/PLAN.md` | 从根目录移动 | 直接复制 |
| `docs/业务文档.md` | 从根目录移动 | 直接复制 |
| `docs/开发文档.md` | 从根目录移动 | 直接复制 |
| `docs/需求文档.md` | 从根目录移动 | 直接复制 |

### 2.2 修改文件 (M)

| 文件路径 | 修改说明 | 影响 | 服务器部署方式 |
|---------|---------|------|---------------|
| `.gitignore` | 添加 `.playwright-mcp/` | 忽略临时文件 | 直接复制 |
| `README.md` | 移除 ScienceClaw 引用，端口修正 5173→5174 | 文档准确性 | 直接复制 |
| `ResearchClaw/backend/main.py` | 添加全局 HTTPException 处理器 | **需要重启 backend** | 复制 + 重启容器 |
| `ResearchClaw/backend/deepagent/shutdown.py` | 修复信号处理线程安全 | **需要重启 backend** | 复制 + 重启容器 |
| `ResearchClaw/backend/deepagent/subagent.py` | 修复字段名、资源限制、JSON错误处理 | **需要重启 backend** | 复制 + 重启容器 |
| `ResearchClaw/frontend/index.html` | 移除 CDN highlight.js | **需要重建 frontend** | 复制 + 重启容器 |
| `ResearchClaw/frontend/src/main.ts` | 导入 highlight.js 并暴露全局 | **需要重建 frontend** | 复制 + 重启容器 |
| `ResearchClaw/websearch/main.py` | 优雅处理缺失 playwright 模块 | **需要重建 websearch** | 复制 + 重建镜像 |

### 2.3 文件重命名 (R100)

| 原路径 | 新路径 | 说明 |
|--------|--------|------|
| `PLAN.md` | `docs/PLAN.md` | 文档整理 |
| `业务文档.md` | `docs/业务文档.md` | 文档整理 |
| `开发文档.md` | `docs/开发文档.md` | 文档整理 |
| `需求文档.md` | `docs/需求文档.md` | 文档整理 |

---

## 三、服务器部署流程

### 3.1 环境信息

- **服务器**: Windows + Docker
- **项目位置**: `D:\project\ResearchClaw`
- **GitLab**: http://192.168.1.241:9980/root/researchclaw
- **访问地址**: http://localhost:5174 (前端)

### 3.2 完整部署命令

```powershell
# 1. 进入项目目录
cd D:\project\ResearchClaw

# 2. 拉取最新代码
git pull

# 3. 重启 websearch（必须重建镜像）
docker-compose stop websearch
docker-compose rm -f websearch
docker-compose build --no-cache websearch
docker-compose up -d websearch

# 4. 重启 backend（代码挂载，只需重启）
docker-compose restart backend

# 5. 重启 frontend（代码挂载，只需重启）
docker-compose restart frontend

# 6. 检查所有服务状态
docker-compose ps

# 7. 查看日志
docker-compose logs -f websearch
docker-compose logs -f backend
docker-compose logs -f frontend
```

### 3.3 关键区别：Rebuild vs Restart

| 服务 | 配置类型 | 代码更新方式 | 命令 |
|------|---------|-------------|------|
| **websearch** | `build: context:` | 代码在镜像内，**必须 rebuild** | `docker-compose build websearch` |
| **backend** | `volumes:` 挂载 | 代码在 volume，**只需 restart** | `docker-compose restart backend` |
| **frontend** | `volumes:` 挂载 | 代码在 volume，**只需 restart** | `docker-compose restart frontend` |

### 3.4 验证部署

```powershell
# 检查后端健康
curl http://localhost:12001/health

# 检查 websearch
curl http://localhost:8068

# 访问前端
start http://localhost:5174
```

---

## 四、问题与解决方案

### 4.1 websearch 启动失败

**现象**: 
```
CalledProcessError: Command '['/usr/local/bin/python3.12', '-m', 'playwright', 
'install', 'chromium']' returned non-zero exit status 1.
```

**原因**: 
- `playwright` 模块未安装（Dockerfile 中注释掉了）
- 启动脚本使用 `check=True`，命令失败时抛出异常

**解决**: 
修改 `main.py`，先检查模块是否存在：
```python
import importlib.util
if importlib.util.find_spec("playwright") is None:
    logger.warning("Playwright 模块未安装，跳过浏览器检查")
else:
    # 尝试安装浏览器
    ...
```

**关键教训**: 
> 使用 `build` 配置的服务，修改代码后**必须 rebuild 镜像**，restart 无效！

### 4.2 frontend build 无反应

**现象**: `docker-compose build --no-cache frontend` 无输出

**原因**: frontend 配置的是 `image:`，不是 `build:`

**解决**: 
- frontend 使用 volume 挂载，代码实时同步
- **只需 `docker-compose restart frontend`**
- 不需要 build

### 4.3 README 端口错误

**现象**: README 写的是 5173，实际是 5174

**原因**: docker-compose.yml 中映射是 `5174:5173`

**解决**: 修正 README.md 为 5174

---

## 五、文件同步清单

### 需要复制的文件（从本地到服务器）

```
# 后端代码（影响 backend 服务）
ResearchClaw/backend/main.py
ResearchClaw/backend/deepagent/shutdown.py
ResearchClaw/backend/deepagent/subagent.py

# websearch 代码（影响 websearch 服务，需 rebuild）
ResearchClaw/websearch/main.py

# 前端代码（影响 frontend 服务）
ResearchClaw/frontend/index.html
ResearchClaw/frontend/src/main.ts

# 文档和版本
README.md
VERSION
CHANGELOG.md
.gitignore

# 文档目录
docs/PLAN.md
docs/业务文档.md
docs/开发文档.md
docs/需求文档.md
```

---

## 六、关键设计决策

### 6.1 为什么 websearch 用 build，frontend 用 image+volumes？

| 服务 | 设计选择 | 原因 |
|------|---------|------|
| websearch | `build` | 需要 Python 环境 + 依赖打包，适合生产部署 |
| frontend | `image` + volumes | 开发模式，Vite 热重载，代码实时同步 |

### 6.2 为什么 highlight.js 从 CDN 改为 npm？

**原因**: CDN 脚本在 Vite 环境下触发 `import.meta` 错误
**解决**: 使用 npm 包管理，import 导入并暴露全局变量

---

## 七、下次部署 checklist

- [ ] `git pull` 拉取最新代码
- [ ] 检查 `git log` 确认提交
- [ ] 识别哪些服务需要 rebuild（有 build 配置）
- [ ] 识别哪些服务只需 restart（有 volumes 挂载）
- [ ] 执行对应的更新命令
- [ ] 检查 `docker-compose ps` 所有服务 running
- [ ] 测试 API 端点
- [ ] 访问前端页面验证

---

## 八、参考链接

- GitLab: http://192.168.1.241:9980/root/researchclaw
- 本地开发: http://localhost:5174
- 后端 API: http://localhost:12001

---

**经验沉淀时间**: 2026-04-01  
**适用版本**: ResearchClaw v0.1.0  
**标签**: #ResearchClaw #Docker #部署 #Windows #GitLab
