# Claude Code Buddy 伴侣系统

> [!INFO]
> 本文档记录 Claude Code 的 `/buddy` 命令功能
> 创建时间: 2026-04-02
> 相关命令: [[Claude Code 架构全景与时序分析]]

---

## 功能概述

`/buddy` 是 Claude Code 中的一个**特性开关控制**的伴侣系统。它在终端界面中添加一个虚拟宠物/生物，出现在输入框旁边，偶尔会在气泡中发表评论。

**功能开关**: `BUDDY` (基于 teaser 窗口逻辑，大约在 2026 年 4 月后启用)

---

## 伴侣属性

### 物种 (Species) — 共 18 种

| 英文名 | 中文名 | 英文名 | 中文名 |
|--------|--------|--------|--------|
| duck | 鸭子 | ghost | 幽灵 |
| goose | 鹅 | axolotl | 蝾螈 |
| blob | 果冻 | capybara | 水豚 |
| cat | 猫 | cactus | 仙人掌 |
| dragon | 龙 | robot | 机器人 |
| octopus | 章鱼 | rabbit | 兔子 |
| owl | 猫头鹰 | mushroom | 蘑菇 |
| penguin | 企鹅 | chonk | 小团子 |
| turtle | 乌龟 | snail | 蜗牛 |

> [!NOTE]
> 原来文档中提到 20 种，但源码中只找到 18 种，可能包括 `chonk` 和 `snail`

### 稀有度 (Rarity)

| 稀有度 | 英文 | 权重 | 颜色 |
|--------|------|------|------|
| 普通 | common | 60% | 灰色 |
| 不普通 | uncommon | 25% | 绿色 |
| 稀有 | rare | 10% | 蓝色 |
| 史诗 | epic | 4% | 青色 |
| 传说 | legendary | 1% | 黄色 |

### 外形属性

#### 眼睛样式 (Eyes)
```
·  ✦  ×  ◉  @  °
```

#### 帽子样式 (Hats)
```
none | crown | tophat | propeller | halo | wizard | beanie | tinyduck
无   | 王冠   | 礼帽   | 螺旋桨   | 光环  | 巫师帽 | 毛线帽  | 小鸭子
```

#### 闪光 (Shiny)
- 1% 概率出现闪光版本

### 属性值 (Stats) — 共 5 项

| 属性 | 含义 |
|------|------|
| DEBUGGING | 调试能力 |
| PATIENCE | 耐心 |
| CHAOS | 混乱度 |
| WISDOM | 智慧 |
| SNARK | 讽刺值 |

---

## 使用方式

### `/buddy pet` — 抚摸

当你抚摸你的伴侣时，会有爱心飘起约 2.5 秒：

```typescript
const PET_BURST_MS = 2500;
const PET_HEARTS = [
  '   ♥    ♥   ',
  '  ♥  ♥   ♥  ',
  ' ♥   ♥  ♥   ',
  '♥  ♥      ♥ ',
  '·    ·   ·  '
];
```

### 伴侣反应

伴侣会观察你的对话，偶尔在气泡中显示反应/评论，10 秒后自动消失。

---

## 界面特性

### 伴侣精灵 (CompanionSprite)

- 在输入框旁边渲染 ASCII 艺术风格的伴侣
- **待机动画**:
  - 大部分时间休息 (第 0 帧)
  - 偶尔动一动 (第 1-2 帧)
  - 罕见的眨眼
- **窄终端支持**: 当列数 < 100 时，收缩为单行脸部显示

### 气泡 (Speech Bubble)

- 显示伴侣的反应/评论
- 约 10 秒后自动淡出
- 文本在 30 个字符处自动换行

### 全屏模式

- 使用 `CompanionFloatingBubble` 覆盖层
- 气泡延伸到滚动区域而不被裁剪

---

## 配置存储

存储在 `settings.json` 中：

```typescript
// 存储在配置中
companion?: StoredCompanion  // 名字、个性、孵化时间
companionMuted?: boolean     // 静音伴侣
```

伴侣由两部分组成：

1. **骨骼 (Bones)** — 确定性：由 `hash(userId)` 派生
   - 物种、稀有度、眼睛、帽子、闪光、属性值

2. **灵魂 (Soul)** — 持久化：用户赋予的名字、个性描述、孵化时间戳

---

## 核心实现细节

### 确定性生成

使用 Mulberry32 伪随机数生成器 (PRNG)，结合 userId 哈希值，确保每次生成的伴侣可复现。

### 缓存机制

Roll 结果会被缓存，避免每次 tick 都重新计算。

### 特性门控

所有伴侣代码都受 `feature('BUDDY')` 检查保护。

### Teaser 宣传期

2026 年 4 月 1-7 日，向新用户展示彩虹色的 `/buddy` 通知提示。

---

## 源码文件

| 文件 | 用途 |
|------|------|
| `restored-src/src/buddy/companion.ts` | 伴侣生成 (roll 系统、物种、属性) |
| `restored-src/src/buddy/types.ts` | 类型定义 (Species, Rarity 等) |
| `restored-src/src/buddy/sprites.ts` | 18 种物种的 ASCII 艺术 |
| `restored-src/src/buddy/CompanionSprite.tsx` | 渲染伴侣的 React 组件 |
| `restored-src/src/buddy/useBuddyNotification.tsx` | 启动时的 teaser 通知 |
| `restored-src/src/buddy/prompt.ts` | 伴侣行为的系统提示词 |

---

## 相关链接

- [[Claude Code 架构全景与时序分析]]
- [[Agent Runtime 术语表]]
- [[给 AI 的标准总提示词]]

---

## 更新日志

| 日期 | 内容 |
|------|------|
| 2026-04-02 | 初始创建，基于源码分析 |
