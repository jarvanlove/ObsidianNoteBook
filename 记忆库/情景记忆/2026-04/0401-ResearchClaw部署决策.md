# ResearchClaw 部署决策记录

> 日期: 2026-04-01  
> 项目: #ResearchClaw  
> 类型: 情景记忆 - 决策记录

---

## 决策背景

用户部署 ResearchClaw 到 Windows Docker 服务器 (192.168.1.241)，需要：
1. 列举所有修改的文件
2. 确定复制到服务器的流程
3. 明确各服务的启动方式

---

## 关键发现

### 发现 1: 两种服务模式

**websearch** 使用 `build` 配置：
```yaml
websearch:
  build:
    context: ./ResearchClaw/websearch
```
- 代码打包在镜像内
- 修改后必须 `docker-compose build`

**frontend/backend** 使用 `image` + `volumes`：
```yaml
frontend:
  image: node:20-bookworm-slim
  volumes:
    - ./ResearchClaw/frontend:/app
```
- 代码通过 volume 挂载
- 修改后只需 `docker-compose restart`

### 发现 2: websearch 启动失败原因

原代码：
```python
subprocess.run([sys.executable, "-m", "playwright", 
               "install", "chromium"], check=True)
```

问题：
- playwright 模块未安装
- `check=True` 导致失败时抛出异常
- 服务无法启动

修复：
```python
import importlib.util
if importlib.util.find_spec("playwright") is None:
    logger.warning("Playwright 模块未安装，跳过浏览器检查")
```

### 发现 3: frontend build 无反应原因

执行 `docker-compose build frontend` 无输出，因为：
- frontend 配置的是 `image:`，不是 `build:`
- 没有 build 配置，build 命令跳过

---

## 最终决策

| 服务 | 更新方式 | 命令 |
|------|---------|------|
| websearch | 重建镜像 | `docker-compose build websearch` |
| backend | 重启容器 | `docker-compose restart backend` |
| frontend | 重启容器 | `docker-compose restart frontend` |

---

## 执行记录

1. ✅ 修复 websearch main.py - 添加模块检查
2. ✅ 修复 README 端口 5173→5174
3. ✅ 提交所有变更到 GitLab
4. ✅ 编写完整部署流程文档

---

## 经验沉淀

- 已写入 `语义记忆/ResearchClaw/Docker部署流程与文件同步经验.md`
- 已写入 `语义记忆/ResearchClaw/Phase1-6功能实现总结.md`

---

**标签**: #ResearchClaw #部署 #决策记录 #Docker
