# ResearchClaw 前端 UI 设计规范

> 版本: 1.0 | 基于当前 dev 分支代码分析
> 更新时间: 2026-04-03

---

## 一、设计理念

**关键词**: 专业、清晰、现代、可信

ResearchClaw 作为科研 AI 平台，设计风格应体现：
- **专业性**: 科研的严谨性和专业性
- **清晰度**: 信息层次分明，易于阅读
- **现代感**: 简洁现代的视觉风格
- **信任感**: 建立用户对 AI 的信任

---

## 二、主题与模式

### 主题系统
- **默认模式**: 浅色模式 (Light Mode)
- **深色模式**: 支持深色切换 (`.dark` class)
- **Tasks 专用主题**: Tasks 页面使用 Sky/Teal 主题

### 核心 CSS 变量文件
| 文件 | 用途 |
|------|------|
| `src/assets/theme.css` | 原始主题变量（Notion 风格） |
| `src/styles/design-system.css` | 设计系统变量（科研蓝+青绿） |
| `tailwind.config.js` | Tailwind 配置 |

---

## 三、色彩系统

### 3.1 主色调 (Primary Blue)

| Token | 色值 | 用途 |
|-------|------|------|
| `primary-50` | `#eff6ff` | 浅色背景、hover 状态 |
| `primary-100` | `#dbeafe` | 选中状态背景 |
| `primary-200` | `#bfdbfe` | 边框 |
| `primary-300` | `#93c5fd` | 图标、链接 hover |
| `primary-400` | `#60a5fa` | 深色模式主色 |
| `primary-500` | `#3b82f6` | 主色调 |
| `primary-600` | `#2563eb` | 主要按钮背景（**常用**） |
| `primary-700` | `#1d4ed8` | 按钮 hover |
| `primary-800` | `#1e40af` | 深色背景 |
| `primary-900` | `#1e3a8a` | 深色文本 |
| `primary-950` | `#172554` | 最深背景 |

### 3.2 辅助色 (Accent Teal)

| Token | 色值 | 用途 |
|-------|------|------|
| `accent-400` | `#2dd4bf` | AI 助手头像渐变色 |
| `accent-500` | `#14b8a6` | 工具调用图标 |
| `accent-600` | `#0d9488` | 工具 hover |

### 3.3 功能色

#### 成功 (Success)
| Token | 色值 | 用途 |
|-------|------|------|
| `success-500` | `#22c55e` | 成功图标 |
| `success-600` | `#16a34a` | 成功文本 |

#### 警告 (Warning)
| Token | 色值 | 用途 |
|-------|------|------|
| `warning-500` | `#f59e0b` | 警告图标 |
| `warning-600` | `#d97706` | 警告文本 |

#### 错误 (Error)
| Token | 色值 | 用途 |
|-------|------|------|
| `error-500` | `#ef4444` | 错误图标、删除按钮 |
| `error-600` | `#dc2626` | 错误文本 |

### 3.4 中性色 (Gray Scale)

| Token | 色值 | 用途 |
|-------|------|------|
| `gray-0` | `#ffffff` | 纯白背景 |
| `gray-50` | `#f8fafc` | 页面背景（Scientific White） |
| `gray-100` | `#f1f5f9` | 卡片背景、hover 背景 |
| `gray-200` | `#e2e8f0` | 边框、分隔线（**常用**） |
| `gray-300` | `#cbd5e1` | placeholder、禁用状态 |
| `gray-400` | `#94a3b8` | 次要文本 |
| `gray-500` | `#64748b` | 三级文本 |
| `gray-600` | `#475569` | 正文文本 |
| `gray-700` | `#334155` | 标题文本 |
| `gray-800` | `#1e293b` | 深色模式背景 |
| `gray-900` | `#0f172a` | 深色模式文字 |

### 3.5 语义化变量

#### 浅色模式 (Light)
```css
--bg-primary: #ffffff;        /* 主背景 */
--bg-secondary: #f8fafc;     /* 页面背景 */
--bg-tertiary: #f1f5f9;      /* 卡片背景 */
--bg-elevated: #ffffff;       /* 提升元素 */
--bg-hover: #f1f5f9;          /* hover 背景 */
--bg-active: #e2e8f0;         /* 激活状态 */

--text-primary: #1e293b;      /* 主文本 */
--text-secondary: #475569;    /* 次要文本 */
--text-tertiary: #64748b;     /* 三级文本 */
--text-disabled: #94a3b8;     /* 禁用文本 */

--border-primary: #e2e8f0;    /* 主边框 */
--border-focus: #3b82f6;      /* 聚焦边框 */
```

---

## 四、字体系统

### 4.1 字体栈

```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```

**优先级**: Inter > 系统字体栈

### 4.2 字号系统

| Token | 数值 | Tailwind | 用途 |
|-------|------|----------|------|
| `--text-xs` | 0.75rem (12px) | `text-xs` | 辅助文本、徽章 |
| `--text-sm` | 0.875rem (14px) | `text-sm` | 正文、次要信息 |
| `--text-base` | 1rem (16px) | `text-base` | 聊天消息 |
| `--text-lg` | 1.125rem (18px) | `text-lg` | 页面标题 |
| `--text-xl` | 1.25rem (20px) | `text-xl` | 大标题 |
| `--text-2xl` | 1.5rem (24px) | `text-2xl` | Hero 标题 |
| `--text-3xl` | 1.875rem (30px) | - | 展示标题 |
| `--text-4xl` | 2.25rem (36px) | - | 大标题 |

### 4.3 字重

| Token | 数值 | 用途 |
|-------|------|------|
| `--font-normal` | 400 | 正文 |
| `--font-medium` | 500 | 标签、次要标题 |
| `--font-semibold` | 600 | 标题、按钮 |
| `--font-bold` | 700 | Logo、重要标题 |

### 4.4 行高

| Token | 数值 | 用途 |
|-------|------|------|
| `--leading-tight` | 1.25 | 标题 |
| `--leading-snug` | 1.375 | 紧凑文本 |
| `--leading-normal` | 1.5 | 正文 |
| `--leading-relaxed` | 1.625 | 聊天消息（宽松阅读） |

---

## 五、间距系统

基于 4px 网格系统：

| Token | 数值 | Tailwind | 用途 |
|-------|------|----------|------|
| `--space-1` | 0.25rem (4px) | `p-1` | 紧凑内边距 |
| `--space-2` | 0.5rem (8px) | `p-2` | 默认内边距 |
| `--space-3` | 0.75rem (12px) | `p-3` | 组件内间距 |
| `--space-4` | 1rem (16px) | `p-4` | 卡片内边距 |
| `--space-5` | 1.25rem (20px) | `p-5` | 大间距 |
| `--space-6` | 1.5rem (24px) | `p-6` | 页面边距 |
| `--space-8` | 2rem (32px) | `p-8` | 大区块间距 |
| `--space-10` | 2.5rem (40px) | - | 区块分隔 |
| `--space-12` | 3rem (48px) | - | 大区块 |

---

## 六、圆角系统

| Token | 数值 | Tailwind | 用途 |
|-------|------|----------|------|
| `--radius-sm` | 0.25rem (4px) | `rounded-sm` | 小元素 |
| `--radius-md` | 0.375rem (6px) | `rounded-md` | 按钮、输入框 |
| `--radius-lg` | 0.5rem (8px) | `rounded-lg` | 卡片、弹窗 |
| `--radius-xl` | 0.75rem (12px) | `rounded-xl` | 大卡片 |
| `--radius-2xl` | 1rem (16px) | `rounded-2xl` | 模态框 |
| `--radius-3xl` | 1.5rem (24px) | - | 聊天输入框 |
| `--radius-full` | 9999px | `rounded-full` | 圆形按钮、徽章 |

**特殊圆角**:
- 聊天输入框: `rounded-[22px]` (右下角/左下角突出)
- 消息气泡: 圆角适中

---

## 七、阴影系统

| Token | 值 | 用途 |
|-------|------|------|
| `--shadow-xs` | `0 1px 2px rgba(0,0,0,0.05)` | 微妙阴影 |
| `--shadow-sm` | `0 1px 3px rgba(0,0,0,0.1)` | 卡片默认 |
| `--shadow-md` | `0 4px 6px rgba(0,0,0,0.1)` | 悬停状态 |
| `--shadow-lg` | `0 10px 15px rgba(0,0,0,0.1)` | 弹窗 |
| `--shadow-inner` | `inset 0 2px 4px rgba(0,0,0,0.05)` | 输入框内阴影 |

**Tailwind 扩展阴影**:
- `shadow-soft`: `0 2px 15px rgba(0,0,0,0.07)` - 柔和卡片
- `shadow-soft-lg`: `0 10px 40px rgba(0,0,0,0.1)` - 柔和弹窗
- `shadow-glow`: `0 0 20px rgba(59,130,246,0.15)` - 聚焦发光

---

## 八、边框系统

### 边框样式
- **默认边框**: `1px solid var(--border-primary)` (#e2e8f0)
- **聚焦边框**: `1px solid var(--border-focus)` (#3b82f6)
- **hover 边框**: `1px solid #cbd5e1` (gray-300)

### 常用边框模式
```html
<!-- 按钮边框 -->
<button class="border border-gray-200 hover:border-gray-300">

<!-- 卡片边框 -->
<div class="rounded-xl border border-gray-200">

<!-- 输入框边框 -->
<input class="border border-gray-200 focus:border-indigo-500">
```

---

## 九、过渡与动画

### 9.1 过渡时长

| Token | 值 | 用途 |
|-------|------|------|
| `--transition-fast` | 150ms ease | 快速交互 (hover) |
| `--transition-normal` | 200ms ease | 默认过渡 |
| `--transition-slow` | 300ms ease | 大元素、展开 |

### 9.2 常用动画

```css
/* 淡入 */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

/* 上滑淡入 */
@keyframes slideUp {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

/* 旋转加载 */
@keyframes spin {
  to { transform: rotate(360deg); }
}

/* 骨架屏闪烁 */
@keyframes shimmer {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

---

## 十、图标系统

### 10.1 图标库
- **Lucide Vue Next**: `lucide-vue-next`
- **常用尺寸**: 16px、20px、24px

### 10.2 图标颜色

| 状态 | 颜色变量 |
|------|----------|
| 默认 | `var(--icon-primary)` (#34322d) |
| 次要 | `var(--icon-secondary)` (#535350) |
| 三级 | `var(--icon-tertiary)` (#858481) |
| 禁用 | `var(--icon-disable)` (#b9b9b7) |
| 品牌色 | `var(--icon-brand)` (#2D5A7B) |

---

## 十一、组件规范

### 11.1 按钮 (Button)

#### 主要按钮
```html
<button class="inline-flex items-center justify-center gap-2 px-4 py-2
  bg-blue-600 text-white rounded-lg font-medium text-sm
  hover:bg-blue-700 active:bg-blue-800
  disabled:opacity-50 disabled:cursor-not-allowed
  transition-all duration-150">
  按钮文本
</button>
```

#### 次要按钮
```html
<button class="inline-flex items-center justify-center gap-2 px-4 py-2
  bg-white text-gray-700 rounded-lg font-medium text-sm
  border border-gray-200
  hover:bg-gray-50 hover:border-gray-300
  transition-all duration-150">
  按钮文本
</button>
```

#### 图标按钮
```html
<button class="size-8 rounded-xl inline-flex items-center justify-center
  border border-gray-200 bg-white
  hover:bg-gray-50 hover:border-gray-300 hover:shadow-sm
  transition-all duration-200">
  <Icon :size="16" class="text-gray-500" />
</button>
```

### 11.2 输入框 (Input)

```html
<input
  class="w-full px-3 py-2 text-sm rounded-lg
    bg-white border border-gray-200
    text-gray-900 placeholder-gray-400
    focus:outline-none focus:ring-2 focus:ring-indigo-500/20 focus:border-indigo-400
    transition-all duration-200"
  placeholder="输入提示..."
/>
```

### 11.3 卡片 (Card)

```html
<div class="bg-white rounded-xl border border-gray-200 p-4
  hover:shadow-md transition-shadow duration-150">
  卡片内容
</div>
```

### 11.4 聊天输入框 (ChatBox)

**特点**:
- 圆角: `rounded-[22px]` (24px)
- 背景: `var(--fill-input-chat)` (#fff)
- 边框: `border border-black/[0.08]`
- 聚焦状态: `focus-within:ring-2 focus-within:ring-indigo-500/15`
- 内边距: `px-4 py-3`

```html
<div class="flex flex-col gap-3 rounded-[22px]
  bg-white py-3 px-4
  border border-black/[0.08]
  focus-within:ring-2 focus-within:ring-indigo-500/15
  focus-within:border-indigo-300/50">
  <!-- 输入框 -->
  <textarea class="..."></textarea>
  <!-- 底部工具栏 -->
  <footer class="flex justify-between">
    <!-- 工具按钮 -->
    <!-- 发送按钮 -->
  </footer>
</div>
```

### 11.5 消息气泡 (Message)

#### 用户消息
- 背景: 渐变或纯色
- 头像: `primary-100` 背景 + `primary-700` 文字

#### AI 助手消息
- 头像: 渐变 `accent-500` → `primary-500`
- 背景: 透明或浅灰

---

## 十二、Z-Index 层级

| 值 | Token | 用途 |
|----|-------|------|
| 1000 | `--z-dropdown` | 下拉菜单 |
| 1020 | `--z-sticky` | 固定元素 |
| 1040 | `--z-modal-backdrop` | 模态背景 |
| 1050 | `--z-modal` | 模态框 |
| 1060 | `--z-popover` | 浮层 |
| 1070 | `--z-tooltip` | 提示 |
| 1080 | `--z-toast` | 通知 |

---

## 十三、滚动条样式

```css
/* Webkit (Chrome, Safari) */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
::-webkit-scrollbar-track {
  background: transparent;
}
::-webkit-scrollbar-thumb {
  background: var(--gray-300);
  border-radius: 3px;
}
::-webkit-scrollbar-thumb:hover {
  background: var(--gray-400);
}

/* Firefox */
scrollbar-width: thin;
scrollbar-color: rgba(0,0,0,0.1) transparent;
```

---

## 十四、特殊页面主题

### Tasks 页面 (Sky/Teal 主题)

```css
.tasks-page-teal {
  --border-input-active: #38bdf8;  /* sky-400 */
  --text-brand: #38bdf8;
  accent-color: #38bdf8;
}
```

---

## 十五、响应式断点

使用 Tailwind 默认断点：
- `sm`: 640px
- `md`: 768px
- `lg`: 1024px
- `xl`: 1280px
- `2xl`: 1536px

---

## 十六、关键设计模式

### 16.1 导航栏 (Navigation Rail)
- 宽度: 60px (收起) / 320px (展开)
- 激活状态: 紫色渐变背景 + 左侧竖线指示器
- 过渡: `width: 300ms ease-in-out`

### 16.2 分享弹窗 (Share Popover)
- 宽度: 380px
- 圆角: `rounded-2xl`
- 选项间距: `gap-3` (12px)
- 选项内边距: `px-3 py-3`

### 16.3 状态徽章
- 圆角: `rounded-full`
- 内边距: `px-2 py-1`
- 字号: `text-xs`

---

## 十七、设计忌讳 (基于昨天教训)

### ❌ 不要做的事
1. **不要**随意添加外层边框包裹 ChatBox
2. **不要**使用深色背景 `bg-[#1e293b]` 在浅色页面上
3. **不要**混用 Notion 风格和科研蓝风格变量
4. **不要**改变原有的交互逻辑
5. **不要**添加多余的容器层级

### ✅ 应该做的事
1. **应该**使用 CSS 变量保持一致性
2. **应该**使用现有的圆角系统
3. **应该**保持阴影的微妙性
4. **应该**使用 `--border-primary` 而非硬编码颜色
5. **应该**参考现有组件的实际代码

---

## 十八、文件对应关系

| 组件/页面 | 文件路径 | 设计要点 |
|-----------|----------|----------|
| ChatBox | `src/components/ChatBox.vue` | 聊天输入框核心组件 |
| ChatPage | `src/pages/ChatPage.vue` | 主聊天页面 |
| HomePage | `src/pages/HomePage.vue` | 首页（无 ChatBox 外边框） |
| LoginPage | `src/pages/LoginPage.vue` | 登录页（浅色卡片） |
| LeftPanel | `src/components/LeftPanel.vue` | 左侧导航 |
| SessionItem | `src/components/SessionItem.vue` | 会话列表项 |

---

*本文档将作为 UI 重新设计的参考标准。修改任何页面时，请先对照此文档确保设计一致性。*
