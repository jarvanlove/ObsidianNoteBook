# Stitch Enlightened Lab UI Design Specification
> Source: stitch_researchclaw_need/enlightened_lab/ | Date: 2026-04-03

## 设计哲学

**The Enlightened Lab** — 科学实验室般精确 + 现代艺术画廊般优雅

核心原则：
- **刻意不对称** — 用留白引导视线聚焦
- **层级表面** — 用色调变化代替线条分隔
- **科学精确感** — 0.2s 微交互动画强化平台可靠性

---

## 色彩系统

### 主色板
| Token | Hex | 用途 |
|-------|-----|------|
| primary | #4a40e0 | 主要操作、激活状态 |
| secondary | #702ae1 | 次要操作、强调 |
| tertiary | #006576 | 第三次强调（青绿） |

### 表面层级（No-Line 规则）
**禁止使用边框**，通过背景色变化区分层级：

| 层级 | Token | Hex | 用途 |
|------|-------|-----|------|
| Level 0 | surface | #f5f7f9 | 底色背景 |
| Level 1 | surface-container-low | #eef1f3 | 大区块 |
| Level 2 | surface-container-lowest | #ffffff | 卡片、聚焦元素 |
| Level 3 | surface-container-high | #dfe3e6 | 按钮 |
| Level 4 | surface-container-highest | #d9dde0 | 最高层级 |

### 文字色
| Token | Hex | 用途 |
|-------|-----|------|
| on-surface | #2c2f31 | 主文字 |
| on-surface-variant | #595c5e | 次要文字 |
| outline | #747779 | 图标 |
| outline-variant | #abadaf | Ghost border（15% 透明度） |

---

## 字体系统

| 角色 | 字体 | 用途 |
|------|------|------|
| Headlines | Manrope | 标题、章节头 |
| Body | Inter | 正文、描述 |
| Labels | Inter | 标签、元数据 |

---

## 布局结构

```
┌──────────────────────────────────────────────────────┐
│ TopNavBar (sticky, glassmorphism: backdrop-blur-xl)  │
├──────────┬───────────────────────────────────────────┤
│ SideNav  │ Main Content (ml-64, max-w-7xl)         │
│ w-64     │                                           │
│ fixed    │ ┌─────────────────────────────────────┐  │
│          │ │ Page Header                          │  │
│ - Brand  │ └─────────────────────────────────────┘  │
│ - Nav    │ ┌─────────────────────────────────────┐  │
│ - Upgrade│ │ Metric Cards Grid                  │  │
│          │ └─────────────────────────────────────┘  │
│          │ ┌─────────────────────────────────────┐  │
│          │ │ Content Grid / Table                │  │
│          │ └─────────────────────────────────────┘  │
└──────────┴───────────────────────────────────────────┘
```

### SideNavBar
- 固定宽度：256px (w-64)
- 背景：bg-slate-50 (接近 surface 色)
- 激活状态：bg-white text-indigo-600 shadow-sm
- 非激活状态：text-slate-500 hover:text-indigo-500

### TopNavBar
```html
<header class="bg-slate-50/80 backdrop-blur-xl shadow-[0_12px_32px_rgba(44,47,49,0.06)] sticky top-0 z-50">
```

---

## 组件系统

### Primary Button
```html
<button class="bg-gradient-to-br from-primary to-secondary text-white px-5 py-2 rounded-lg font-bold shadow-lg shadow-primary/20 hover:scale-[1.02] active:scale-95 transition-all">
```

### Card
```html
<div class="bg-surface-container-lowest rounded-xl p-6 shadow-[0_12px_32px_rgba(44,47,49,0.04)] border border-white/50 hover:shadow-xl transition-all duration-300">
```

### Form Input
```html
<input class="w-full bg-surface-container-lowest border-[1.5px] border-outline-variant/15 rounded-lg px-4 py-3 focus:outline-none focus:border-primary focus:ring-4 focus:ring-primary/10 transition-all">
```

### Toggle Switch
```html
<button class="w-10 h-5 bg-primary rounded-full relative">
  <div class="absolute right-0.5 top-0.5 w-4 h-4 bg-white rounded-full shadow-sm"></div>
</button>
```

### Status Badge
```html
<span class="inline-flex items-center gap-1 px-2.5 py-0.5 rounded-full text-[11px] font-bold bg-emerald-100 text-emerald-700">
  <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>
  Success
</span>
```

---

## 图标系统

Material Symbols Outlined
```html
<link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:wght,FILL@100..700,0..1&display=swap" rel="stylesheet"/>

<!-- 标准 -->
<span class="material-symbols-outlined">icon</span>

<!-- 填充 -->
<span class="material-symbols-outlined" style="font-variation-settings: 'FILL' 1;">icon</span>
```

导航图标：
- Chat: `chat_bubble`
- Skills: `auto_awesome`
- Tools: `construction`
- Tasks: `assignment`
- Team: `group`
- DAG: `account_tree`

---

## 动画与过渡

| 时长 | 用途 |
|------|------|
| 150ms | 即时反馈 |
| 200ms | 标准过渡 |
| 300ms | 卡片悬停 |
| 500ms | 错开动画 |

```css
transition-all duration-200 ease-in-out

/* 悬停缩放 */
.hover\:scale-\[1\.02\]:hover { transform: scale(1.02); }

/* 点击缩放 */
.active\:scale-95:active { transform: scale(0.95); }
```

---

## 阴影系统

| 名称 | 值 | 用途 |
|------|-----|------|
| Card Shadow | 0 12px 32px rgba(44,47,49,0.04) | 标准卡片 |
| Hover Shadow | 0 20px 50px rgba(74,64,224,0.08) | 悬停卡片 |
| Nav Shadow | 0 12px 32px rgba(44,47,49,0.06) | 顶部导航 |

---

## Glassmorphism

```html
<div class="bg-surface-container-lowest/80 backdrop-blur-2xl rounded-xl">
```

---

## 响应式断点

| 断点 | 最小宽度 | 行为 |
|------|----------|------|
| Mobile | < 640px | 堆叠布局，隐藏侧栏 |
| Tablet | 640px+ | 2列网格 |
| Desktop | 768px+ | 完整侧栏，3列网格 |
| Large | 1024px+ | 扩展导航 |
| XL | 1280px+ | 最大内容宽度 |

---

## DO's and DON'Ts

### Do
- 使用不对称内边距创造编辑感
- 使用 surface-tint (#4a40e0) 2-3% 透明度覆盖背景
- 优先使用色调变化而非线条分隔

### Don't
- 禁止使用纯黑 (#000000) — 使用 on-surface (#2c2f31)
- 禁止使用 100% 不透明边框
- 禁止堆砌数据 — 增加间距而非缩小字体

---

## Tailwind 配置

```javascript
colors: {
  "primary": "#4a40e0",
  "secondary": "#702ae1",
  "tertiary": "#006576",
  "surface": "#f5f7f9",
  "surface-container-low": "#eef1f3",
  "surface-container-lowest": "#ffffff",
  "surface-container-high": "#dfe3e6",
  "on-surface": "#2c2f31",
  "on-surface-variant": "#595c5e",
  "outline": "#747779",
  "outline-variant": "#abadaf",
},
fontFamily: {
  "headline": ["Manrope"],
  "body": ["Inter"],
}
```

---

## 与当前项目差异

| 方面 | 当前项目 | Enlightened Lab |
|------|----------|-----------------|
| Primary | Blue-500 (#3b82f6) | Indigo (#4a40e0) |
| Accent | Teal (#14b8a6) | Violet (#702ae1) |
| 边框 | Gray-200 | 无（色调变化） |
| 卡片 | 白色背景 | surface-container-lowest (#fff) |

---

*文档版本: 1.0*
*来源: 12个 Stitch UI 原型分析*
