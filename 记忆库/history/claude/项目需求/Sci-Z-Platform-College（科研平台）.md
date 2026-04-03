# Sci-Z-Platform-College（科研平台）

**科研项目管理平台**

📁 路径：`C:\Work\note\CursorWorkSpace\SALL\ZOE_Link_AI\Sci-Z-Platform-College`

🔧 功能：
- 申报管理
- 仪表板统计
- 首页展示
- 多语言i18n

---

### 2026-03-19

**需求：** 扩展红头文件解析，增加解析部门、红头文件发布时间字段

- 实现：修改RedHeaderFileParseResp DTO，新增department和publishTime字段，扩展Dify工作流调用
- 技术：Java, Dify Workflow, Qwen-VL-Max

**需求：** 增加文件预览功能，用户可预览上传的附件

- 实现：使用现有文件上传接口和预览组件，在删除按钮旁增加预览按钮
- 技术：Vue, Element UI Plus

**需求：** 增加链接解析方式，解析微信公众号或政府网站链接，自动填充部门、研究方向、研究课题等字段

- 实现：新增ParseUrlReq和链接解析接口，使用Jsoup解析HTML，提取文章内容后调用AI填充表单
- 技术：Java, Jsoup, Dify Workflow

**需求：** 修复首页农林Logo与其他Logo大小不一致问题

- 实现：调整CSS样式统一Logo容器高度和object-fit设置
- 技术：Vue, SCSS

**需求：** 修复menu.home翻译问题

- 实现：在所有语言包中补充menu.home翻译键值对
- 技术：Vue i18n

**需求：** 修复workflow_status显示running而不是completed的问题

- 实现：修改DeclarationWorkflowTask确保流程完成后更新状态
- 技术：Java

**需求：** 仪表板4个区域点击跳转项目列表并传递状态参数

- 实现：修改DashboardStatCard添加router.push传递status参数，ProjectList从URL读取参数
- 技术：Vue, Vue Router

