# Claude Code 任务执行策略总览

> 最后更新: 2026-03-17
> 基于: Agency-Agents-Zh (176角色) + Superpowers Skills + MCP Plugins + GSD

---

## 快速启动

```
"我需要 [做什么]" → 系统自动识别场景 → 激活角色 → 选择工具
```

### 角色激活方式

```bash
# 自然语言激活（推荐）
"以前端开发者的角色帮我写个React组件"
"扮演产品经理分析这个需求"
"作为小红书运营帮我写文案"
"切换到安全审查员模式检查代码"

# 系统会自动匹配 ~/.claude/agents/ 中的角色定义
```

---

## 1. 工具清单

### 1.1 核心工作流: Spec-Workflow + GSD (已安装)

#### Spec-Workflow (Spec驱动开发) - **新项目推荐**

**官方**: https://github.com/Pimzino/claude-code-spec-workflow

结构化工作流程：**需求 → 设计 → 任务 → 实现**

| 命令 | 用途 | 适用场景 |
|---------|-----|---------|
| `/spec-steering-setup` | 项目初始化 | 创建 product.md, tech.md, structure.md |
| `/spec-create <name>` | 完整Spec流程 | 新功能开发，自动走完4个阶段 |
| `/spec-execute <id>` | 执行任务 | 手动执行指定任务 |
| `/spec-status` | 查看进度 | 显示当前Spec状态 |
| `/spec-list` | 列出所有 | 显示所有Spec和Bug |
| `/bug-create <name>` | Bug修复流程 | 创建Bug修复文档 |
| `/bug-analyze` | 分析Bug | 查找根因 |
| `/bug-fix` | 修复Bug | 实现解决方案 |
| `/bug-verify` | 验证修复 | 确认Bug已解决 |
| `/bug-status` | Bug状态 | 显示Bug修复进度 |

**Spec-Workflow 工作流**:
```
新功能: /spec-steering-setup → /spec-create feature-name → /spec-execute
Bug修复: /bug-create bug-name → /bug-analyze → /bug-fix → /bug-verify
```

#### GSD (Get Shit Done) - **备选方案**

| 命令 | 用途 | 适用场景 |
|---------|-----|---------|
| `/gsd:new-project` | 初始化新项目 | 项目启动，创建 PROJECT.md |
| `/gsd:plan-phase` | 创建阶段计划 | 制定详细计划，创建 PLAN.md |
| `/gsd:execute-phase` | 执行阶段计划 | 按计划实施，自动并行执行 |
| `/gsd:debug` | 系统调试 | Bug排查，持续状态追踪 |
| `/gsd:progress` | 检查进度 | 状态检查，路由下一步 |
| `/gsd:verify-work` | 验证工作 | 功能验证，UAT |
| `/gsd:quick` | 快速任务 | 原子提交，跳过可选代理 |

**选择建议**:
- **新功能开发**: 优先使用 Spec-Workflow (`/spec-create`)
- **Bug修复**: 优先使用 Spec-Workflow Bug流程 (`/bug-create`)
- **大型项目**: GSD 的里程碑管理 (`/gsd:new-project`)
- **快速任务**: GSD Quick (`/gsd:quick`) 或 Superpowers

### 1.2 Plugins (已安装)

| 插件 | 用途 | 适用场景 |
|-----|-----|---------|
| superpowers | 超级能力集 | 计划执行、调试、TDD、代码审查 |
| feature-dev | 功能开发 | 新功能实现、代码探索 |
| claude-mem | 记忆系统 | 跨会话记忆、智能搜索 |
| github | GitHub集成 | 代码托管、PR管理 |
| figma | Figma集成 | UI设计协作 |
| code-review | 代码审查 | 代码质量检查 |
| code-simplifier | 代码简化 | 重构优化 |
| context7 | 文档查询 | 查官方文档、API用法 |
| playwright | 浏览器自动化 | E2E测试、截图 |
| ralph-loop | Ralph循环 | 持续迭代 |
| commit-commands | Git提交命令 | 版本控制 |

### 1.3 Skills (已安装)

| Skill | 用途 | 适用场景 | 调用方式 |
|------|-----|---------|---------|
| adversarial-review | 对抗性审查 | 代码审查 | /adversarial-review |
| documentation-writer | 文档编写 | 技术文档 | /documentation-writer |
| find-skills | 技能查找 | 查找补充工具 | /find-skills |
| frontend-design | 前端设计 | UI实现 | Skill:frontend-design |
| humanizer | 文本人性化 | 文案优化 | /humanizer |
| paper-reading | 论文阅读 | 学术研究 | /paper-reading |
| planning-with-files | 文件规划 | 项目规划 | /planning-with-files |
| ui-ux-pro-max | UI/UX设计 | 产品设计 | /ui-ux-pro-max |
| shadcn-ui-setup | UI快速搭建 | Next.js+shadcn | Skill:shadcn-ui-setup |
| docker-containerization | Docker容器化 | 部署、CI/CD | Skill:docker-containerization |
| supabase-mcp-integration | Supabase集成 | 数据库、Auth | Skill:supabase-mcp-integration |

### 1.4 Superpowers Skills

| Skill | 用途 | 适用场景 | 调用方式 |
|------|-----|---------|---------|
| brainstorming | 头脑风暴 | 创意工作前 | Skill:superpowers:brainstorming |
| writing-plans | 编写计划 | 多步骤任务规划 | Skill:superpowers:writing-plans |
| executing-plans | 执行计划 | 按计划实施 | Skill:superpowers:executing-plans |
| test-driven-development | TDD开发 | 功能/bugfix | Skill:superpowers:test-driven-development |
| systematic-debugging | 系统调试 | Bug排查 | Skill:superpowers:systematic-debugging |
| verification-before-completion | 完成前验证 | 声称完成前 | Skill:superpowers:verification-before-completion |
| requesting-code-review | 请求代码审查 | 任务完成后 | Skill:superpowers:requesting-code-review |
| using-git-worktrees | Git工作树 | 功能隔离开发 | Skill:superpowers:using-git-worktrees |
| subagent-driven-development | 子代理开发 | 并行独立任务 | Skill:superpowers:subagent-driven-development |

---

## 2. 多场景任务分配策略

### 场景矩阵

| 用户说 | 识别场景 | 激活角色 | 工具组合 |
|-------|---------|---------|---------|
| "帮我做个App/网站" | 独立开发0-1 | 完整角色链 | planning + frontend-design + feature-dev |
| "写个登录页/组件" | 前端开发 | 前端开发者 | Skill:frontend-design |
| "设计数据库/API" | 后端架构 | 后端架构师 | Skill:supabase-mcp-integration |
| "部署到服务器" | DevOps | DevOps工程师 | Skill:docker-containerization |
| "review代码" | 代码审查 | 代码审查员 | Skill:adversarial-review |
| "有bug帮我看看" | 调试 | 调试专家 | Skill:superpowers:systematic-debugging |
| "写测试用例" | 测试 | 测试工程师 | MCP:playwright |
| "小红书文案" | 内容营销 | 小红书运营 | Skill:humanizer |
| "抖音脚本" | 短视频 | 抖音策略师 | - |
| "微信小程序" | 小程序开发 | 微信小程序开发者 | Skill:frontend-design |

---

## 3. 详细场景指南

### 场景A: 独立开发0-1（完整流程）

适用: "帮我做个产品"、"从0开发一个App"

**首选方案: Spec-Workflow** (推荐用于新功能开发)

```
/spec-steering-setup → 项目初始化 (product.md, tech.md, structure.md)
    ↓
/spec-create feature-name "功能描述" → 完整Spec流程
    [自动生成 requirements.md → design.md → tasks.md]
    ↓
/spec-execute 1 → 执行任务 (或使用生成的任务命令)
    ↓
验证 → 完成
```

**备选方案 1: GSD 工作流** (适合大型项目)

```
/gsd:new-project → 初始化项目 (PROJECT.md)
    ↓
/gsd:plan-phase → 规划第一阶段 (PLAN.md)
    ↓
/gsd:execute-phase → 执行开发
    ↓
/gsd:verify-work → 验证功能
    ↓
/gsd:complete-milestone → 完成里程碑
```

**备选方案 2: Superpowers 工作流** (适合小到中型项目)

| 阶段 | 激活角色 | 交付物 | 工具/Skill |
|-----|---------|-------|-----------|
| 想法验证 | 产品经理 + UX研究员 | PRD初稿、用户画像 | planning-with-files |
| 产品设计 | UI设计师 + 架构师 | 设计稿、技术架构 | frontend-design, ui-ux-pro-max |
| 技术选型 | 后端架构师 + 前端开发者 | 技术方案 | context7 |
| 开发实现 | 前端开发者 + 后端开发者 | 功能代码 | feature-dev, superpowers |
| 代码审查 | 代码审查员 | 审查报告 | adversarial-review |
| 测试上线 | 测试工程师 + DevOps | 测试报告、生产环境 | playwright, docker-containerization |

**Superpowers 执行流程**:
```
1. Skill:superpowers:brainstorming → 探索需求和设计
2. Skill:planning-with-files → 创建项目计划
3. Skill:frontend-design → UI设计
4. Skill:superpowers:writing-plans → 编写技术实施计划
5. Skill:feature-dev 或 superpowers:executing-plans → 开发实现
6. Skill:adversarial-review → 代码审查
7. MCP:playwright → E2E测试
```

---

### 场景B: 单功能开发

适用: "帮我写个登录页面"、"实现一个API"

```
用户: "帮我写个登录页面"
↓
识别: 前端功能开发
↓
激活: 前端开发者 (engineering-frontend-developer.md)
↓
工具: Skill:frontend-design + Skill:shadcn-ui-setup
↓
审查: 代码审查员 + Skill:adversarial-review
```

**快捷命令**:
```bash
# 前端组件
"以前端开发者的角色，用React+TypeScript写一个带表单验证的登录组件"

# 后端API
"以后端架构师的角色，设计一个用户认证的REST API，包含JWT token"

# UI设计
"以UI设计师的角色，设计一个简洁的登录页面，要求现代化风格"
```

---

### 场景C: Bug修复

适用: "这个API有性能问题"、"页面崩溃了"

**首选方案: Spec-Workflow Bug流程** (推荐)

```
/bug-create issue-name "Bug描述" → 创建Bug文档
    ↓
/bug-analyze → 分析根因
    ↓
/bug-fix → 实现解决方案
    ↓
/bug-verify → 验证修复
    ↓
完成
```

**备选方案 1: GSD Debug** (复杂Bug)

```
/gsd:debug → 启动调试模式
    ↓
系统化诊断 → 根因分析
    ↓
修复 → 验证 → 完成
```

**备选方案 2: Superpowers 调试**

```
用户: "这个API有性能问题"
↓
识别: 性能优化任务
↓
激活: 后端架构师 + 调试专家
↓
工具: Skill:superpowers:systematic-debugging
```

**执行流程**:
```
1. Skill:superpowers:systematic-debugging → 系统诊断
2. 激活 engineering-database-optimizer → 数据库分析
3. 修复 → 测试 → Skill:adversarial-review → 完成
```

---

### 场景D: 代码审查

适用: "帮我review这段代码"、"检查代码质量"

```
用户: "帮我review这段代码"
↓
识别: 代码审查
↓
激活: 代码审查员 (engineering-code-reviewer.md)
↓
工具: Skill:adversarial-review
```

**执行流程**:
```
1. 激活 engineering-code-reviewer
2. Skill:adversarial-review → 多维度审查
3. 输出: 审查报告 + 改进建议
```

---

### 场景E: 中国本土营销

适用: "帮我写个小红书文案"、"抖音内容规划"

```
用户: "帮我写个小红书文案"
↓
识别: 小红书内容
↓
激活: 小红书运营专家 (marketing-xiaohongshu-operator.md)
↓
工具: Skill:humanizer
```

**角色矩阵**:

| 平台 | 激活角色 | 交付物 |
|-----|---------|-------|
| 小红书 | marketing-xiaohongshu-operator | 种草笔记、内容日历 |
| 抖音 | marketing-douyin-strategist | 视频脚本、投流策略 |
| 快手 | marketing-kuaishou-strategist | 短视频方案 |
| B站 | marketing-bilibili-strategist | UP主合作方案 |
| 知乎 | marketing-zhihu-strategist | 问答内容、长文 |
| 微信 | marketing-wechat-operator | 公众号文章、私域方案 |
| 百度 | marketing-baidu-seo-specialist | SEO优化方案 |
| 电商 | marketing-china-ecommerce-operator | 运营策略 |
| 直播 | marketing-livestream-commerce-coach | 直播脚本、话术 |

---

### 场景F: AI功能开发

适用: "集成AI功能"、"开发MCP服务器"

| 阶段 | 激活角色 | 工具 |
|-----|---------|------|
| 需求分析 | 产品经理 | planning-with-files |
| 提示工程 | specialized-prompt-engineer | - |
| 模型集成 | engineering-ai-engineer | claude-api skill |
| MCP开发 | specialized-mcp-builder | - |
| 数据修复 | engineering-ai-data-remediation-engineer | - |

---

### 场景G: 微信小程序开发

适用: "开发小程序"、"小程序页面"

```
激活: engineering-wechat-mini-program-developer
工具: Skill:frontend-design
框架: 微信原生 / Taro / uni-app
```

---

### 场景H: 学术/研究任务

适用: "读这篇论文"、"整理文献"

```
激活: academic-study-planner / academic-anthropologist
工具: Skill:paper-reading
```

---

## 4. 任务类型速查表

| 任务类型 | 推荐角色 | 主要工具 | 备用工具 |
|---------|---------|---------|---------|
| 新建项目 | 产品经理 + 架构师 | planning-with-files | superpowers:brainstorming |
| 功能开发 | 前端/后端开发者 | feature-dev | superpowers:executing-plans |
| UI设计 | UI设计师 | frontend-design | ui-ux-pro-max |
| UX设计 | UX研究员 | ui-ux-pro-max | - |
| 代码审查 | 代码审查员 | adversarial-review | code-review plugin |
| 调试Bug | 调试专家 | superpowers:systematic-debugging | feature-dev |
| 写文档 | 文档写作者 | documentation-writer | - |
| 测试 | 测试工程师 | playwright | superpowers:TDD |
| 部署 | DevOps工程师 | docker-containerization | - |
| 架构设计 | 后端架构师 | superpowers:writing-plans | planning-with-files |
| 代码探索 | - | claude-mem smart-explore | feature-dev:explore |
| 数据库设计 | 后端架构师 | supabase-mcp-integration | - |
| 前端脚手架 | 前端开发者 | shadcn-ui-setup | - |
| 小红书内容 | 小红书运营 | humanizer | - |
| 抖音内容 | 抖音策略师 | - | - |
| 微信小程序 | 小程序开发者 | frontend-design | - |
| PRD编写 | 产品经理 | planning-with-files | - |
| 安全审查 | 安全工程师 | - | adversarial-review |

---

## 5. 推荐执行模式

### 模式1: 简单任务（< 30分钟）

```
激活角色 → 直接执行 → 快速验证
```

### 模式2: 快速任务（30分钟-2小时）

**推荐: Spec-Workflow 快速创建**
```
/spec-create quick-feature "快速功能描述"
```
或 GSD Quick:
```
/gsd:quick "任务描述"
```
或 Superpowers:
```
Skill:superpowers:brainstorming → 激活角色 → 执行 → Skill:adversarial-review
```

### 模式3: 中大型任务（> 2小时）- **推荐 Spec-Workflow**

**Spec-Workflow 标准工作流** (首选):
```
/spec-steering-setup → /spec-create feature-name → /spec-execute
```

**GSD 备选方案** (适合复杂里程碑):
```
/gsd:new-project → /gsd:plan-phase → /gsd:execute-phase → /gsd:verify-work
```

**Superpowers 备选方案**:
```
Skill:superpowers:brainstorming → Skill:planning-with-files → Skill:superpowers:writing-plans → Skill:superpowers:executing-plans → Skill:adversarial-review → Skill:superpowers:verification-before-completion
```

### 模式4: 调试任务 - **推荐 Spec-Workflow Bug流程**

```
/bug-create issue-name → /bug-analyze → /bug-fix → /bug-verify
```

**备选: GSD Debug**
```
/gsd:debug → 诊断 → 修复 → 验证
```

---

## 6. 已安装工具概览

| 工具 | 类型 | 状态 | 用途 |
|-----|------|------|-----|
| Spec-Workflow | 项目管理 | ✅ 已安装 | Spec驱动开发，需求→设计→任务→实现 |
| GSD | 项目管理 | ✅ 已安装 | 里程碑管理，阶段规划执行 |
| Agency-Agents | 角色系统 | ✅ 已安装 | 176个中文角色定义 |
| Superpowers | 技能集合 | ✅ 已安装 | 计划、执行、调试、TDD |
| shadcn-ui-setup | 前端技能 | ✅ 已安装 | UI快速搭建 |
| supabase-mcp-integration | 后端技能 | ✅ 已安装 | 数据库、Auth |
| docker-containerization | DevOps技能 | ✅ 已安装 | Docker部署 |

---

## 7. 快速命令参考

### 角色激活
```bash
# 自然语言（推荐）
"以前端开发者的角色..."
"扮演产品经理..."
"作为小红书运营..."
```

### Skill 调用
```bash
# 查找技能
/find-skills <关键词>

# 使用技能
Skill:<skill_name>
```

### Spec-Workflow 命令
```bash
# 项目初始化（首次使用）
/spec-steering-setup

# 新功能开发
/spec-create feature-name "功能描述"

# 执行任务
/spec-execute 1

# 查看Spec状态
/spec-status

# 列出所有Spec
/spec-list

# Bug修复流程
/bug-create bug-name "Bug描述"
/bug-analyze
/bug-fix
/bug-verify
/bug-status
```

### GSD 命令
```bash
# 项目初始化
/gsd:new-project

# 规划阶段
/gsd:plan-phase

# 执行阶段
/gsd:execute-phase

# 调试
/gsd:debug

# 快速任务
/gsd:quick "任务描述"

# 验证工作
/gsd:verify-work

# 查看进度
/gsd:progress

# 其他命令
/gsd:help  # 查看所有命令
```

### Superpowers 技能
```bash
# 头脑风暴
Skill:superpowers:brainstorming

# 编写计划
Skill:superpowers:writing-plans

# 执行计划
Skill:superpowers:executing-plans

# TDD开发
Skill:superpowers:test-driven-development

# 系统调试
Skill:superpowers:systematic-debugging

# 完成前验证
Skill:superpowers:verification-before-completion
```

### MCP 使用
```bash
# Context7 文档查询
mcp__context7__resolve-library-id
mcp__context7__query-docs

# Playwright 测试
mcp__playwright__browser_navigate
mcp__playwright__browser_snapshot

# GitHub 操作
mcp__github__create_pull_request
mcp__github__create_issue

# Claude Memory
mcp__claude-mem__smart-explore
mcp__claude-mem__mem-search
```

---

## 8. 持续更新机制

1. **角色更新**: 定期检查 agency-agents-zh 上游更新
2. **技能更新**: `npx skills update` 或重新安装
3. **插件更新**: 通过 Claude Code 设置更新
4. **文档更新**: 每季度回顾并更新本文件

---

## 附录: Agency-Agents 完整列表

### Engineering (20+ 角色)
- engineering-frontend-developer (前端开发者)
- engineering-backend-architect (后端架构师)
- engineering-ai-engineer (AI工程师)
- engineering-devops-automator (DevOps工程师)
- engineering-security-engineer (安全工程师)
- engineering-code-reviewer (代码审查员)
- engineering-wechat-mini-program-developer (微信小程序开发者)
- engineering-mobile-app-builder (移动应用开发者)
- engineering-database-optimizer (数据库优化师)
- engineering-senior-developer (资深开发者)
- engineering-software-architect (软件架构师)
- engineering-data-engineer (数据工程师)
- engineering-embedded-firmware-engineer (嵌入式工程师)
- engineering-solidity-smart-contract-engineer (智能合约工程师)
- engineering-dingtalk-integration-developer (钉钉集成开发者)
- engineering-feishu-integration-developer (飞书集成开发者)
- ... (更多)

### Design (8+ 角色)
- design-ui-designer (UI设计师)
- design-ux-researcher (UX研究员)
- design-system-lead (设计系统负责人)
- design-brand-guardian (品牌守护者)
- design-image-prompt-engineer (图像提示工程师)
- design-inclusive-visuals-specialist (包容性视觉专家)

### Product (5+ 角色)
- product-manager (产品经理)
- product-owner (产品负责人)
- product-analyst (产品分析师)

### Marketing (25+ 角色，含中国原创)
- marketing-xiaohongshu-operator (小红书运营)
- marketing-xiaohongshu-specialist (小红书专家)
- marketing-douyin-strategist (抖音策略师)
- marketing-kuaishou-strategist (快手策略师)
- marketing-bilibili-strategist (B站策略师)
- marketing-zhihu-strategist (知乎策略师)
- marketing-wechat-operator (微信运营)
- marketing-wechat-official-account (公众号运营)
- marketing-private-domain-operator (私域流量运营)
- marketing-baidu-seo-specialist (百度SEO专家)
- marketing-china-ecommerce-operator (中国电商运营)
- marketing-cross-border-ecommerce (跨境电商运营)
- marketing-livestream-commerce-coach (直播电商教练)
- marketing-content-creator (内容创作者)
- marketing-growth-hacker (增长黑客)
- ... (更多)

### Testing (5+ 角色)
- testing-automation-engineer (自动化测试工程师)
- testing-qa-specialist (QA专家)
- testing-security-tester (安全测试工程师)

### Specialized (15+ 角色)
- specialized-prompt-engineer (提示词工程师)
- specialized-mcp-builder (MCP构建器)
- specialized-gov-digital-presales (政务数字化售前顾问)
- academic-study-planner (学习规划师)
- academic-anthropologist (人类学研究者)
- academic-geographer (地理学研究者)
- academic-historian (历史学研究者)
- ... (更多)

### Sales (10+ 角色)
- sales-solution-architect (解决方案架构师)
- sales-technical-sales (技术销售)
- ... (更多)

### Strategy (10+ 角色)
- strategy-business-strategist (商业战略师)
- ... (更多)

### Project Management (5+ 角色)
- project-manager (项目经理)
- project-agile-coach (Agile教练)
- ... (更多)

### 其他分类
- Game Development (游戏开发)
- Finance (金融)
- HR (人力资源)
- Legal (法律)
- Support (客户支持)
- Academic (学术)
- Supply Chain (供应链)
- Spatial Computing (空间计算)
- Paid Media (付费媒体)

---

**总计**: 176个角色，覆盖工程、设计、产品、营销、管理、学术等全领域。
