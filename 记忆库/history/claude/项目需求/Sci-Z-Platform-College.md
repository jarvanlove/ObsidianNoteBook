# Sci-Z-Platform-College

**Sci-Z高校平台**

📁 路径：`C:/Work/note/CursorWorkSpace/SALL/ZOE_Link_AI/Sci-Z-Platform-College`

🔧 功能：
- 前端服务
- 后端服务

---

### 2026-03-19

**需求：** 农林这个和其他不一样大小

- 实现：调整PartnersSection.vue中logo框的CSS样式，使农林logo与其他logo保持一致高度
- 技术：Vue/SCSS

**需求：** 如图，这个按钮中希望增加鼠标悬浮交互特效

- 实现：修改HeroSection.vue按钮样式，添加hover背景颜色变化效果
- 技术：Vue/SCSS

**需求：** 给点背景颜色变化难道不好吗？

- 实现：为按钮添加hover背景颜色变化交互特效
- 技术：Vue/SCSS

**需求：** 首页中剩下的2个立即体验按钮希望也可以使用如图6所示的按钮

- 实现：统一CtaSection.vue和HomeFooter.vue中的按钮样式与HeroSection.vue一致
- 技术：Vue/SCSS

**需求：** 图7中的这个地球看着不真实啊

- 实现：修改地球动画效果使其更真实
- 技术：Vue/CSS动画

**需求：** menu.home怎么没翻译成首页？哪里出了问题

- 实现：修复国际化文件中menu.home的翻译配置
- 技术：Vue i18n

**需求：** ko-KR.js在报错

- 实现：修复ko-KR.js语法错误（多余的逗号）
- 技术：JavaScript

**需求：** 突然发现这个地方不需要首页按钮的请去掉

- 实现：从ToCSidebar.vue中移除首页按钮
- 技术：Vue

**需求：** 图10所示为数据库数据，流程都执行完成了为什么workflow_status还是running而不是completed

- 实现：检查并修改DeclarationWorkflowTask.java中workflow_status状态更新逻辑
- 技术：Java/Spring Boot

**需求：** 图14，这4个区域希望可以点击并跳转（用户鼠标悬浮时候给出交互特效），且点击哪一块要带着参数

- 实现：修改DashboardStatCard.vue和Dashboard.vue，添加点击跳转功能并传递状态参数到ProjectList.vue
- 技术：Vue/Vue Router

**需求：** toc-ui-beautify、toc、toc-wjw 3个分支代码一样吗？

- 实现：同步代码到toc和toc-wjw分支，合并toc-ui-beautify的修改
- 技术：Git


### 2026-03-17

**需求：** 先分析一下我们这个项目前后端启动后，前端页面打开的默认页面是哪个？

- 实现：需要分析项目代码确定默认入口页面
- 技术：待分析

**需求：** 根据领导的要求，我们需要做一个首页，参考借鉴 https://discovery.intern-ai.org.cn/home 和 https://www.bohrium.com/intro ，我更中意第一个：https://www.bohrium.com/intro ，这个参考借鉴知道是别人的设计，布局，风格、样式等，内容还是要根据我们的项目来

- 实现：待提供建设性执行计划（用户要求暂不写代码）
- 技术：参考 Bohrium 网站风格

**需求：** 首页新增好了之后，希望服务启动就展示首页，用火狐点击首页上某个按钮再跳转到当前的默认页面，希望你要做到联动性、无缝集成，不可以影响到现在的任何东西

- 实现：待实施
- 技术：路由集成

**需求：** 除了首页需要新增集成，领导对于目前系统的页面、部分按钮 觉得设计不好看。监护不智能，体验较差，所以还需要对系统的所有页面、按钮、交互、进行优化

- 实现：待提供全面优化方案
- 技术：UI/UX优化

**需求：** 1-4 是个大工程，希望你可以给出建设性执行计划和意见，保证最终修改优化后的页面能够抓住用户眼球、突出科研主题，暂时不要编写任何代码

- 实现：正在规划中
- 技术：规划阶段


### 2026-03-19

**需求：** 如图，上海农林这个和其他不一样大小

- 实现：修改PartnersSection.vue中Logo图片样式，调整object-fit和尺寸
- 技术：Vue3 + SCSS

**需求：** 1.首页中 剩下的 2个 立即体验按钮 希望也可以使用如图6所示的按钮，也就是3个地方使用同一个 2.图7中的这个地球 看着不真实啊

- 实现：统一HeroSection.vue、CtaSection.vue、HomeFooter.vue中的按钮样式
- 技术：Vue3 + SCSS

**需求：** 1.图10所示为 数据库数据，流程都执行完成了 为什么workflow_status 还是 running 而不是 completed 2.图11所示为该条数据的前端展示。因为图1中的处理 所以千吨啊一直显示处理中(86% 实际上应该已完成 100%) 3.我说一下这个逻辑：新建申报页面 点击 提交申报后 需要后端处理 申报详情页面会调用后端接口进行展示，所以你要检查一下 新建申报页面 点击 提交申报后 后端处理逻辑

- 实现：修改DeclarationWorkflowTask.java中工作流状态更新逻辑，确保申报完成后status更新为completed
- 技术：Java Spring Boot

**需求：** 1.如图14，这4个区域希望可以点击并跳转到项目列表页面（用户鼠标悬浮时候给出交互特效），且点击哪一块要带着参数 2.如图13刚刚修改的代码和这个需求 在 toc 分支 toc-wjw 分支也需要合并的

- 实现：修改DashboardStatCard.vue和Dashboard.vue添加点击跳转功能，并在ProjectList.vue添加URL参数读取逻辑
- 技术：Vue3

**需求：** 1.如图2所示，是目前的新建申报页面，用户上传了红头文件后会解析这个附件，调用的自己搭建的dify工作流（目前工作流只返回了 研究方向 研究课题 研究课题 3个字段，现在老板想要附件解析好之后可以将 部门、红头文件发布时间也解析出来并填充到相应的表单中 2.图2中文档解析请检查以下后端是否上传到文件服务器？因为还需要再删除按钮左边增加一个 预览按钮 3.图2所示目前只有一种方式进行红头文件上传，老板希望再增加一种方式，根据连接解析

- 实现：开始探索项目上下文，准备进行需求澄清
- 技术：Vue3 + Java Spring Boot + Dify工作流


### 2026-03-19

**需求：** 如图，上海农林这个和其他不一样大小

- 实现：修改PartnersSection.vue中logo显示样式，调整图片大小统一
- 技术：Vue/SCSS

**需求：** 如图，这个按钮中 希望增加鼠标悬浮交互特效

- 实现：修改HeroSection.vue添加hover背景色变化效果
- 技术：Vue/SCSS

**需求：** 1.首页中 剩下的 2个 立即体验按钮 希望也可以使用如图6所示的按钮，也就是3个地方使用同一个 2.图7中的这个地球 看着不真实啊

- 实现：统一CtaSection.vue、HeroSection.vue、HomeFooter.vue按钮样式；优化地球动画效果
- 技术：Vue/SCSS

**需求：** 1.如图，menu.home 怎么没翻译成 首页？哪里出了问题 2.ko-KR.js 在报错

- 实现：修复zh-CN.js中menu.home翻译键值，修复ko-KR.js语法错误（多余逗号）
- 技术：JavaScript/i18n

**需求：** 突然发现 这个地方不需要首页按钮的 请去掉

- 实现：从ToCSidebar.vue中移除首页按钮相关代码
- 技术：Vue

**需求：** 1.图10所示为 数据库数据，流程都执行完成了 为什么workflow_status 还是 running 而不是 completed 2.图11所示为该条数据的前端展示。因为图1中的处理 所以千吨啊一直显示处理中(86% 实际上应该已完成 100%)

- 实现：修改DeclarationWorkflowTask.java中工作流状态更新逻辑
- 技术：Java

**需求：** 1.如图14，这4个区域希望可以点击并跳转到项目列表页面（用户鼠标悬浮时候给出交互特效），且点击哪一块要带着参数 2.当前分支是为了设计首页 单独从toc 分支拉的，最终都需要合并到 toc toc-wjw 也就意味着（当前分支、toc、toc-wjw 代码要一样）

- 实现：修改DashboardStatCard.vue和Dashboard.vue添加点击跳转功能，传递项目状态参数；同步代码到toc和toc-wjw分支
- 技术：Vue/JavaScript

**需求：** 1.如图2所示，是目前的新建申报页面，用户上传了红头文件后会解析这个附件，调用的自己搭建的dify工作流（目前工作流只返回了 研究方向 研究课题 研究课题 3个字段，现在老板想要附件解析好之后可以 将 部门、红头文件发布时间也解析出来并填充到相应的表单中

- 实现：扩展RedHeaderFileParseResp.java，新增department和publishDate字段；修改Dify工作流调用
- 技术：Java

**需求：** 2.图2中文档解析请检查以下后端是否上传到文件服务器？因为还需要再删除按钮左边增加一个 预览按钮

- 实现：检查FileServiceImpl.java上传逻辑，在DeclarationCreate.vue添加预览按钮
- 技术：Java/Vue

**需求：** 3.图2所示目前只有一种方式进行红头文件上传，老板希望再增加一种方式，根据连接解析，将链接贴进来后点击解析后可以自动将 部门、红头文件发布时间、研究方向、研究课题、研究领域 进行填充，且可以看到链接解析后的文章

- 实现：新增ParseUrlReq.java和链接解析接口，使用jsoup解析URL内容，提取文章正文
- 技术：Java/Vue

**需求：** 1. 文档上传、 链接解析 文字有点小且未选中时候字体颜色希望可以和部门字体颜色大小一致 2.链接解析 中 解析按钮 鼠标悬浮 按钮主题有问题 3.原文内容 字体和和部门字体颜色大小一致

- 实现：修改DeclarationCreate.vue中文字大小和颜色样式
- 技术：Vue/SCSS


### 2026-03-19

**需求：** 如图，上海农林这个和其他不一样大小

- 实现：调整PartnersSection.vue中农林logo的尺寸样式
- 技术：Vue/CSS

**需求：** 希望增加鼠标悬浮交互特效

- 实现：给HeroSection.vue按钮添加hover背景颜色变化
- 技术：Vue/CSS

**需求：** 首页中剩下的2个立即体验按钮希望也可以使用如图6所示的按钮

- 实现：统一CtaSection.vue和HomeFooter.vue的按钮样式与HeroSection一致
- 技术：Vue/CSS

**需求：** menu.home怎么没翻译成首页

- 实现：修复ToCSidebar.vue中menu.home的翻译key
- 技术：i18n/Vue

**需求：** ko-KR.js在报错

- 实现：修复ko-KR.js语法错误-删除多余的逗号
- 技术：JavaScript

**需求：** 这个地方不需要首页按钮的请去掉

- 实现：从ToCSidebar.vue移除首页按钮
- 技术：Vue

**需求：** 仪表板4个区域希望可以点击并跳转到项目列表页面，且点击哪一块要带着参数

- 实现：修改DashboardStatCard.vue添加点击跳转和参数传递，修改ProjectList.vue读取URL参数
- 技术：Vue/Vue Router

**需求：** 当前分支、toc、toc-wjw代码要一样

- 实现：将代码合并到toc和toc-wjw分支
- 技术：Git

**需求：** 红头文件解析要增加部门和红头文件发布时间

- 实现：扩展RedHeaderFileParseResp.java DTO，修改Dify工作流调用
- 技术：Java/Spring Boot

**需求：** 文档解析需要上传到文件服务器，增加预览按钮

- 实现：调用现有文件上传接口，获取文件URL使用预览组件
- 技术：Vue/Element UI Plus

**需求：** 增加链接解析方式，根据链接自动填充部门、发布时间、研究方向、研究课题、研究领域，显示链接解析后的文章内容

- 实现：新增ParseUrlReq.java，新增链接解析接口，使用jsoup解析网页内容
- 技术：Java/Spring Boot/jsoup


### 2026-03-21

**需求：** 自然 是长期做法，我刚刚在服务器 powershell 执行 npx playwright --version 也显示 1.58.2 说明没问题，问题出现代码解析路径 所以需要你长期处理 修改代码 避免部署路径依赖（达成jar包部署到服务器）

- 实现：修改了 DeclarationServiceImpl.java 和 crawl_url.py 脚本，处理了部署路径依赖问题
- 技术：Java + Python + Playwright

**需求：** 以前 si-z-server/scripts 文件夹 以及文件夹下的文件crawl_url.py 还有用吗？

- 实现：将脚本移动到 src/main/resources/scripts/ 目录下
- 技术：Java Resources


### 2026-03-21

**需求：** 本地执行npx playwright正常，服务器报错无法运行脚本

- 实现：服务器PowerShell执行策略问题，需修改代码避免部署路径依赖
- 技术：Java/PowerShell

**需求：** 需要长期处理避免部署路径依赖达成jar包部署

- 实现：将crawl_url.py移到resources/scripts目录，修改DeclarationServiceImpl.java调用方式
- 技术：Java


### 2026-03-21

**需求：** 自然是我刚刚在服务器执行npx playwright --version也显示1.58.2说明没问题，问题出现代码解析路径所以需要你长期处理修改代码避免部署路径依赖（达成jar包部署到服务器）

- 实现：修改DeclarationServiceImpl.java中的路径解析逻辑，改为相对路径或可配置路径
- 技术：Java Spring Boot


### 2026-03-23

**需求：** 目前 gitlab 上有2个分支 toc-hyy toc-zxt 这2个分支分别对应2个实习生（都是基于toc分支创建），暂时需要他们再自己的分支上去学习熟悉该项目，上周toc分支有信代码提交，所以需要这2个实习生拉取代码并push到自己的远程分支

- 实现：提供了git fetch + git merge或git pull命令让实习生从toc分支拉取最新代码到自己分支
- 技术：Git

**需求：** 先让2个实习生 分别拉取 toc分支的最新代码到自己的分支 出现冲突解决（以toc分支为准），然后push到自己分支

- 实现：提供了完整的git命令：git fetch origin、git checkout toc-xxx、git merge origin/toc、解决冲突、git push
- 技术：Git

**需求：** 提交后需要将代码合并到toc分支 并切换回 toc-wjw

- 实现：执行了git merge、git push推送到远程toc和toc-wjw分支
- 技术：Git


### 2026-03-23

**需求：** 老板反馈 系统的首页最下面那个模块 公式logo + 智能驱动 · 科研赋能 + 联系我们 +关注我们   其中  公司logo 因为这一模块的颜色问题  导致无法看清 也无法看清logo的文字 ，检查并修复

- 实现：多次修改HomeFooter.vue的logo样式，但用户不满意审美，后恢复并重新设计
- 技术：Vue.js, CSS

**需求：** 点击 实践 按钮 然后选择 学院项目管理智能体 点击仪表板 对应的页面 进入页面后 项目延期预警 这个图表不能正常的显示 必须刷新页面才可以 检查并修复这个问题

- 实现：修复了DelayWarningChart.vue、DashboardPieChart.vue、EfficiencyChart.vue的图表刷新问题，使用nextTick确保数据加载后渲染
- 技术：Vue.js, ECharts

**需求：** 1.如图，不是说有个 tab切换吗？ 没看到呢，这个是科研平台 你刚刚不是移动了什么文件吗

- 实现：更新了ToCSidebar.vue，添加了AI对话页面的tab切换逻辑
- 技术：Vue.js, Vue Router


### 2026-03-23

**需求：** 点击实践按钮然后选择学院项目管理智能体点击仪表板对应的页面进入页面后项目延期预警这个图表不能正常的显示必须刷新页面才可以检查并修复这个问题

- 实现：修改DelayWarningChart.vue添加watch监听route变化，使用nextTick确保数据加载后重新渲染图表
- 技术：Vue3 + ECharts

**需求：** 系统的首页最下面那个模块公式logo + 智能驱动 · 科研赋能 + 联系我们 +关注我们 其中公司logo因为这一模块的颜色问题导致无法看清

- 实现：修改HomeFooter.vue，调整logo样式增强对比度
- 技术：Vue3 + CSS


### 2026-03-23

**需求：** 目前 gitlab 上有2个分支 toc-hyy  toc-zxt  这2个分支分别对应2个实习生（都是基于toc分支创建），暂时需要他们再自己的分支上去学习熟悉该项目，上周toc分支有信代码提交，所以需要这2个实习生拉取代码并push到自己的远程分支，请告诉我他们的操作命令

- 实现：提供了git fetch、git merge、git push命令让实习生同步toc分支最新代码到自己的远程分支
- 技术：Git

**需求：** 那暂时先不需要了 ，先让2个实习生 分别拉取 toc分支的最新代码刀自己的分支 出现冲突解决（以toc分支为准），然后push到自己分支 给出操作命令

- 实现：提供了git fetch origin、git merge origin/toc --strategy-option theirs、git push命令
- 技术：Git

**需求：** 1.提交2.提交后需要将代码合并到toc分支 并切换回 toc-wjw

- 实现：用户执行了git commit和git push操作，成功将toc-wjw分支的修改推送到gitlab
- 技术：Git

**需求：** 老板反馈 系统的首页最下面那个模块 公式logo + 智能驱动 · 科研赋能 + 联系我们 +关注我们   其中  公司logo 因为这一模块的颜色问题  导致无法看清 也无法看清logo的文字 ，检查并修复

- 实现：多次修改HomeFooter.vue的logo显示样式，从深色背景到浅色背景，从图片到文字，最终确保logo可见
- 技术：Vue + CSS

**需求：** 点击 实践 按钮 然后选择 学院项目管理智能体 点击仪表板 对应的页面 进入页面后 项目延期预警 这个图表不能正常的显示 必须刷新页面才可以 检查并修复这个问题

- 实现：修改了DelayWarningChart.vue、EfficiencyChart.vue、DashboardPieChart.vue的mounted和watch逻辑，解决图表需要刷新才能显示的问题
- 技术：Vue + ECharts

**需求：** 启动这个项目

- 实现：启动了前端(8707)和后端(8808)服务
- 技术：Vite + Spring Boot


### 2026-03-23

**需求：** 帮我分别启动前后端服务

- 实现：启动了前端(8707)和后端(8808)服务
- 技术：Spring Boot + Vue

**需求：** 老板反馈 系统的首页最下面那个模块 公式logo + 智能驱动 · 科研赋能 + 联系我们 +关注我们 其中 公司logo 因为这一模块的颜色问题 导致无法看清 也无法看清logo的文字，检查并修复

- 实现：多次修改HomeFooter.vue样式，调整logo清晰度和背景对比
- 技术：Vue + CSS

**需求：** 点击实践按钮 然后选择学院项目管理智能体 点击仪表板 对应的页面 进入页面后 项目延期预警这个图表不能正常的显示 必须刷新页面才可以 检查并修复这个问题

- 实现：修改DelayWarningChart.vue、EfficiencyChart.vue、DashboardPieChart.vue的图表初始化逻辑，解决数据加载问题
- 技术：Vue + ECharts

**需求：** 1.立即做：Token传递 2.在Sci-Z-Platform加ResearchAgent.vue页面

- 实现：创建了auth.py中间件提取Token，创建了researchAgentApi.js、AgentChat.vue、ResearchAgent.vue并集成到路由
- 技术：FastAPI + Vue

**需求：** 他们已经拉取了项目并且切换到自己的分支，所以2个实习生分别需要执行的的命令是什么

- 实现：提供了git fetch origin、git merge origin/toc、git push命令
- 技术：Git

**需求：** 先让2个实习生分别拉取toc分支的最新代码到自己的分支 出现冲突解决（以toc分支为准），然后push到自己分支 给出操作命令

- 实现：提供git fetch、git checkout toc-hyy/toc-zxt、git merge origin/toc、git push命令
- 技术：Git


### 2026-03-25

**需求：** 分别启动前后端服务

- 实现：尝试启动前后端服务，前端启动失败（node命令问题），后端连接失败（未授权401错误）
- 技术：Vite/Node.js + Java/Spring Boot

**需求：** PROGRESS.md这个是前天下午让你根据gstack来实现当前项目而做的一个文档 你可以检查一下 然后根据你更新的最新gstack来完成项目开发对接

- 实现：用户要求检查PROGRESS.md并根据最新gstack完成项目开发对接
- 技术：待确定


### 2026-03-25

**需求：** 分别启动前后端服务

- 实现：尝试启动前���(8707端口)和后端(8808端口)服务，但遇到连接被拒绝错误
- 技术：Vite + Java Spring Boot

**需求：** 告诉我后端启动命令

- 实现：提供后端启动命令：mvn spring-boot:run -Dspring-boot.run.arguments=--server.port=8808
- 技术：Maven


### 2026-03-25

**需求：** 分别启动前后端服务

- 实现：尝试启动前端(8707)和后端(8808)服务，但遇到连接被拒绝和node命令问题
- 技术：Vite, Java Spring Boot


### 2026-03-25

**需求：** 分别启动前后端服务

- 实现：尝试启动后端(端口8808)和前端(端口8707)服务，过程中遇到node/vite命令问题和端口连接问题
- 技术：Java Spring Boot + Vue/Vite

**需求：** 关闭8808端口进程

- 实现：成功关闭PID 30016的进程
- 技术：Windows taskkill命令


### 2026-03-25

**需求：** 分别启动前后端服务

- 实现：尝试启动前端(8707)和后端(8808)服务
- 技术：Vite + Java/Spring


### 2026-03-25

**需求：** 分别启动前后端服务

- 实现：尝试启动前端（端口8707）和后端（端口8808），遇到连接被拒
- 技术：Java(Vite+Spring)


### 2026-03-25

**需求：** 分别启动前后端服务

- 实现：尝试启动前端vite服务(8707)和后端服务(8808)，但遇到端口连接问题
- 技术：Vite/Spring Boot

