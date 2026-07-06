# 🎨 设计规范 (Design System)

> **📌 模板说明**: 本文档定义产品的视觉设计系统，包括色彩、字体、间距、阴影、动效等所有视觉要素。是设计和开发团队的共同参考。AI 填写时请替换所有 `<!-- FILL -->` 占位符。

---

## 🗂️ 元数据

| 字段 | 内容 |
|------|------|
| 📦 产品名称 | <!-- FILL --> |
| 🔖 版本 | <!-- FILL --> |
| 📅 日期 | <!-- FILL: YYYY-MM-DD --> |
| 🎨 设计工具 | <!-- FILL: Figma / Sketch / 无 --> |

---

## 💡 1. 设计原则

| # | 🏷️ 原则 | 📝 描述 | 💡 示例 |
|---|---------|---------|---------|
| 1 | <!-- FILL: 🎯 一致性 --> | <!-- FILL: 相同功能使用相同视觉语言 --> | 所有按钮圆角统一 6px |
| 2 | <!-- FILL: 📊 层次感 --> | <!-- FILL: 通过明暗/大小建立信息层级 --> | 标题 > 副标题 > 正文 |
| 3 | <!-- FILL: 🌊 留白 --> | <!-- FILL: 给内容呼吸空间，避免拥挤 --> | 卡片内边距不低于 16px |
| 4 | <!-- FILL: ⚡ 即时反馈 --> | <!-- FILL: 每个操作都有视觉响应 --> | 按钮 hover 150ms 过渡 |
| 5 | <!-- FILL: ♿ 可访问性 --> | <!-- FILL: 所有用户都能无障碍使用 --> | 文字对比度 ≥ 4.5:1 |

---

## 🎨 2. 色彩系统

### 2.1 🔵 品牌色

| 名称 | 🖌️ Hex | RGB | 用途 |
|------|--------|-----|------|
| Primary（主色）| <!-- FILL: #5B7FFF --> | <!-- FILL: rgb(91,127,255) --> | 主按钮、链接、焦点环 |
| Secondary（辅色）| <!-- FILL: #FF7B72 --> | <!-- FILL: rgb(255,123,114) --> | 危险操作、删除、警告 |
| Accent（强调色）| <!-- FILL: #E3B341 --> | <!-- FILL: rgb(227,179,65) --> | 收藏、未保存状态 |
| Success（成功色）| <!-- FILL: #3FB950 --> | <!-- FILL: rgb(63,185,80) --> | 保存成功、完成状态 |

### 2.2 🌙 深色主题 (Dark Theme)

```css
:root[data-theme="dark"] {
  /* 🏗️ 背景层级（由深到浅）*/
  --bg-primary:    <!-- FILL: #0D1117 -->;  /* 最底层背景 */
  --bg-secondary:  <!-- FILL: #161B22 -->;  /* 侧边栏/卡片 */
  --bg-tertiary:   <!-- FILL: #1C2128 -->;  /* 输入框/下拉 */
  --bg-elevated:   <!-- FILL: #21262D -->;  /* 弹窗/悬浮层 */

  /* 📝 文字层级 */
  --text-primary:   <!-- FILL: #E6EDF3 -->;  /* 标题、正文 */
  --text-secondary: <!-- FILL: #8B949E -->;  /* 副文本、描述 */
  --text-tertiary:  <!-- FILL: #484F58 -->;  /* 占位符、禁用 */

  /* 🔲 边框 */
  --border-default: <!-- FILL: #30363D -->;
  --border-muted:   <!-- FILL: #21262D -->;
  --border-strong:  <!-- FILL: #484F58 -->;

  /* 🎨 品牌色 */
  --color-primary:  <!-- FILL: #5B7FFF -->;
  --color-danger:   <!-- FILL: #FF7B72 -->;
  --color-warning:  <!-- FILL: #E3B341 -->;
  --color-success:  <!-- FILL: #3FB950 -->;
  --color-info:     <!-- FILL: #58A6FF -->;
}
```

### 2.3 ☀️ 浅色主题 (Light Theme)

```css
:root[data-theme="light"] {
  --bg-primary:     <!-- FILL: #FFFFFF -->;
  --bg-secondary:   <!-- FILL: #F6F8FA -->;
  --bg-tertiary:    <!-- FILL: #EAEEF2 -->;
  --bg-elevated:    <!-- FILL: #FFFFFF -->;

  --text-primary:   <!-- FILL: #24292F -->;
  --text-secondary: <!-- FILL: #57606A -->;
  --text-tertiary:  <!-- FILL: #8C959F -->;

  --border-default: <!-- FILL: #D0D7DE -->;
  --border-muted:   <!-- FILL: #EAEEF2 -->;
}
```

---

## 🔤 3. 字体系统

### 3.1 字体族

```css
/* 📖 正文 UI 字体 */
--font-sans: <!-- FILL: 'Inter', 'PingFang SC', 'Microsoft YaHei', sans-serif -->;

/* 💻 等宽/代码字体 */
--font-mono: <!-- FILL: 'JetBrains Mono', 'Fira Code', 'Cascadia Code', monospace -->;
```

### 3.2 字号梯度

| 🏷️ Token | 📏 大小 | 📐 行高 | ⚡ 字重 | 📝 用途 |
|----------|---------|---------|---------|---------|
| `--text-xs` | 12px | 16px | 400 | 辅助信息、时间戳 |
| `--text-sm` | 14px | 20px | 400 | 正文、标签、侧边栏 |
| `--text-base` | 16px | 24px | 400 | 编辑器正文 |
| `--text-lg` | 18px | 28px | 500 | 卡片标题、小标题 |
| `--text-xl` | 20px | 28px | 600 | 面板标题 |
| `--text-2xl` | 24px | 32px | 600 | 页面标题 |
| `--text-3xl` | 30px | 36px | 700 | 大标题 |
| `--text-4xl` | 36px | 40px | 700 | 首屏展示标题 |

---

## 📐 4. 间距系统

> 📏 **基准单位: 4px**（所有间距均为 4 的倍数）

| 🏷️ Token | 📏 值 | 📝 典型用途 |
|----------|------|------------|
| `--space-0.5` | 2px | 最紧凑间距（图标与文字） |
| `--space-1` | 4px | 图标内边距 |
| `--space-1.5` | 6px | 小组件内边距 |
| `--space-2` | 8px | 列表项间距、按钮内边距（竖向） |
| `--space-3` | 12px | 输入框内边距 |
| `--space-4` | 16px | **卡片内边距**（最常用） |
| `--space-5` | 20px | 区块内间距 |
| `--space-6` | 24px | 区块间距 |
| `--space-8` | 32px | 区块外边距 |
| `--space-10` | 40px | 大区块间距 |
| `--space-12` | 48px | 页面级内边距 |
| `--space-16` | 64px | 章节间距 |

---

## 🔘 5. 圆角系统

| 🏷️ Token | 📏 值 | 📝 用途 |
|----------|------|---------|
| `--radius-sm` | 4px | Tag、Badge、小标签 |
| `--radius-md` | 6px | 按钮、输入框、菜单项 |
| `--radius-lg` | 8px | 卡片、弹窗、下拉菜单 |
| `--radius-xl` | 12px | 大卡片、侧边栏 |
| `--radius-2xl` | 16px | 模态框 |
| `--radius-full` | 9999px | 圆形头像、Pill 标签 |

---

## 🌫️ 6. 阴影系统

```css
--shadow-sm:   0 1px 2px rgba(0, 0, 0, 0.12);              /* 悬浮元素 */
--shadow-md:   0 4px 6px rgba(0, 0, 0, 0.15);              /* 卡片 */
--shadow-lg:   0 10px 15px rgba(0, 0, 0, 0.2);             /* 弹窗 */
--shadow-xl:   0 20px 25px rgba(0, 0, 0, 0.25);            /* 全屏弹窗 */
--shadow-glow: 0 0 20px rgba(91, 127, 255, 0.3);           /* 品牌发光效果 */
--shadow-inset: inset 0 1px 2px rgba(0, 0, 0, 0.1);        /* 输入框凹陷 */
```

---

## 🏷️ 7. 边框系统

| 🏷️ Token | 规范 | 📝 用途 |
|----------|------|---------|
| `--border-default` | `1px solid #30363D` | 默认边框（卡片、输入框） |
| `--border-strong` | `1px solid #484F58` | 强调边框（hover 状态） |
| `--border-subtle` | `1px solid #21262D` | 次要边框（分割线） |
| `--border-brand` | `1px solid #5B7FFF` | 品牌边框（焦点、选中） |

---

## 🔷 8. 图标规范

| 属性 | 规范 |
|------|------|
| 🎨 图标库 | <!-- FILL: Lucide Icons（推荐）/ Heroicons / Tabler Icons --> |
| 📏 标准尺寸 | 16px（小）/ 20px（中）/ 24px（大） |
| 🎨 颜色 | 继承 `currentColor`，随文字颜色变化 |
| ✏️ 线宽 | 1.5px（默认）/ 2px（粗体强调） |
| 🖱️ 可点击图标 | padding 8px，hover 背景色 |

---

## ⚡ 9. 动效规范

| 🏷️ 动画类型 | ⏱️ 时长 | 📐 缓动函数 | 🎬 触发条件 |
|------------|---------|------------|------------|
| 微交互（hover/active） | 150ms | `ease-out` | 鼠标交互 |
| 元素过渡（展开/收起） | 200ms | `cubic-bezier(0.4, 0, 0.2, 1)` | 状态切换 |
| 面板动画（侧边栏/弹窗） | 250ms | `ease-out` | 打开/关闭 |
| 页面切换 | 200ms | `ease-in-out` | 路由变化 |
| Toast 入场 | 300ms | `spring(1, 80, 10, 0)` | 通知出现 |
| 大型动画（入场） | 500ms+ | `ease-out` | 首屏加载 |

> ⚠️ **动效原则**: 动画服务于用户，不炫技。所有动画 < 500ms，尊重 `prefers-reduced-motion`。

---

## 📱 10. 响应式断点

| 🏷️ Token | 📏 值 | 📱 适用设备 | 布局变化 |
|----------|------|------------|---------|
| `--bp-sm` | 480px | 小手机 | 工具栏精简 |
| `--bp-md` | 768px | 手机/小平板 | 侧边栏覆盖层 |
| `--bp-lg` | 1024px | 平板横屏 | 侧边栏变窄 240px |
| `--bp-xl` | 1280px | 笔记本 | 标准三栏布局 |
| `--bp-2xl` | 1536px | 大显示器 | 最大宽度限制 |

---

## ♿ 11. 可访问性

| 📋 标准 | ✅ 要求 |
|---------|---------|
| 文字对比度 | ≥ 4.5:1（正文）/ ≥ 3:1（大字/图形） |
| 焦点指示器 | 可见的 `focus-visible` ring，2px 品牌色 |
| 键盘导航 | 所有可交互元素支持 Tab / Enter / Escape |
| 屏幕阅读器 | 语义化 HTML + `aria-label` + `role` 属性 |
| 色觉无障碍 | 不纯粹依赖颜色传达信息，配合图标/文字 |
| 动效无障碍 | 支持 `prefers-reduced-motion: reduce` |

---

> **✅ 质量检查**: ✓ 色彩有深浅两套主题 ✓ 字体有完整梯度 ✓ 间距系统基于 4px 基准 ✓ 动效规范明确 ✓ 无障碍标准完整
