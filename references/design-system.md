# 设计规范 (Design System)

> **模板说明**: 本文档定义产品的视觉设计系统，包括色彩、字体、间距、阴影、动效等所有视觉要素。是设计和开发团队的共同参考。

---

## 元数据

| 字段 | 内容 |
|------|------|
| 产品名称 | <!-- FILL --> |
| 版本 | <!-- FILL --> |
| 日期 | <!-- FILL: YYYY-MM-DD --> |
| 设计工具 | <!-- FILL: Figma / Sketch / 无 --> |

---

## 1. 设计原则

| # | 原则 | 描述 | 示例 |
|---|------|------|------|
| 1 | <!-- FILL: 例如 一致性 --> | <!-- FILL: 相同功能使用相同视觉语言 --> | 所有按钮圆角统一 8px |
| 2 | <!-- FILL: 例如 层次感 --> | <!-- FILL: 通过明暗/大小建立信息层级 --> | 标题 > 副标题 > 正文 |
| 3 | <!-- FILL: 例如 留白 --> | <!-- FILL: 给内容呼吸空间 --> | 卡片内边距不低于 16px |
| 4 | <!-- FILL: 例如 反馈 --> | <!-- FILL: 每个操作都有即时反馈 --> | 按钮 hover/active 状态变化 |
| 5 | <!-- FILL: 例如 可访问性 --> | <!-- FILL: 所有用户都能使用 --> | 文字对比度 ≥ 4.5:1 |

---

## 2. 色彩系统

### 2.1 品牌色

| 名称 | Hex | RGB | 用途 |
|------|-----|-----|------|
| <!-- FILL: Primary --> | <!-- FILL: #5B7FFF --> | <!-- FILL: rgb(91,127,255) --> | <!-- FILL: 主按钮、链接 --> |
| <!-- FILL: Secondary --> | <!-- FILL --> | <!-- FILL --> | <!-- FILL --> |
| <!-- FILL: Accent --> | <!-- FILL --> | <!-- FILL --> | <!-- FILL: 强调、CTA --> |

### 2.2 深色主题

```css
:root {
  /* 背景层级 */
  --bg-primary:    <!-- FILL: #0D1117 -->;  /* 最底层背景 */
  --bg-secondary:  <!-- FILL: #161B22 -->;  /* 侧边栏/卡片 */
  --bg-tertiary:   <!-- FILL: #1C2128 -->;  /* 输入框/下拉 */
  --bg-elevated:   <!-- FILL: #21262D -->;  /* 弹窗/悬浮 */

  /* 文字层级 */
  --text-primary:   <!-- FILL: #E6EDF3 -->;
  --text-secondary: <!-- FILL: #8B949E -->;
  --text-tertiary:  <!-- FILL: #484F58 -->;

  /* 边框 */
  --border-default:  <!-- FILL: #30363D -->;
  --border-muted:    <!-- FILL: #21262D -->;

  /* 品牌 */
  --color-primary:   <!-- FILL: #5B7FFF -->;
  --color-danger:    <!-- FILL: #FF7B72 -->;
  --color-warning:   <!-- FILL: #E3B341 -->;
  --color-success:   <!-- FILL: #3FB950 -->;
  --color-info:      <!-- FILL: #58A6FF -->;
}
```

### 2.3 浅色主题

<!-- OPTIONAL: 同上结构，替换为浅色值 -->

### 2.4 功能色

| 名称 | Hex | 用途 |
|------|-----|------|
| <!-- FILL: Success --> | <!-- FILL: #3FB950 --> | <!-- FILL: 成功、完成 --> |
| <!-- FILL: Warning --> | <!-- FILL: #E3B341 --> | <!-- FILL: 警告、注意 --> |
| <!-- FILL: Danger --> | <!-- FILL: #FF7B72 --> | <!-- FILL: 错误、删除 --> |
| <!-- FILL: Info --> | <!-- FILL: #58A6FF --> | <!-- FILL: 提示、帮助 --> |

---

## 3. 字体系统

### 3.1 字体族

```css
/* 正文 */
--font-sans: <!-- FILL: 'Inter', 'PingFang SC', 'Microsoft YaHei', sans-serif -->;

/* 代码 */
--font-mono: <!-- FILL: 'JetBrains Mono', 'Fira Code', 'Cascadia Code', monospace -->;
```

### 3.2 字号梯度

| 标签 | 大小 | 行高 | 字重 | 用途 |
|------|------|------|------|------|
| `--text-xs` | <!-- FILL: 12px --> | <!-- FILL: 16px --> | <!-- FILL: 400 --> | <!-- FILL: 辅助信息 --> |
| `--text-sm` | <!-- FILL: 14px --> | <!-- FILL: 20px --> | <!-- FILL: 400 --> | <!-- FILL: 正文/标签 --> |
| `--text-base` | <!-- FILL: 16px --> | <!-- FILL: 24px --> | <!-- FILL: 400 --> | <!-- FILL: 正文 --> |
| `--text-lg` | <!-- FILL: 18px --> | <!-- FILL: 28px --> | <!-- FILL: 500 --> | <!-- FILL: 小标题 --> |
| `--text-xl` | <!-- FILL: 20px --> | <!-- FILL: 28px --> | <!-- FILL: 600 --> | <!-- FILL: 标题 --> |
| `--text-2xl` | <!-- FILL: 24px --> | <!-- FILL: 32px --> | <!-- FILL: 600 --> | <!-- FILL: 大标题 --> |
| `--text-3xl` | <!-- FILL: 30px --> | <!-- FILL: 36px --> | <!-- FILL: 700 --> | <!-- FILL: 页面标题 --> |
| `--text-4xl` | <!-- FILL: 36px --> | <!-- FILL: 40px --> | <!-- FILL: 700 --> | <!-- FILL: 展示标题 --> |

---

## 4. 间距系统

> 基准单位: **4px**

| Token | 值 | 用途 |
|-------|-----|------|
| `--space-0.5` | 2px | 紧凑间距 |
| `--space-1` | 4px | 图标与文字间距 |
| `--space-1.5` | 6px | 小组件内间距 |
| `--space-2` | 8px | 列表项间距 |
| `--space-3` | 12px | 输入框内边距 |
| `--space-4` | 16px | 卡片内边距 |
| `--space-5` | 20px | 区块内间距 |
| `--space-6` | 24px | 区块间距 |
| `--space-8` | 32px | 区块外间距 |
| `--space-10` | 40px | 大区块间距 |
| `--space-12` | 48px | 页面级间距 |
| `--space-16` | 64px | 章节间距 |

---

## 5. 圆角系统

| Token | 值 | 用途 |
|-------|-----|------|
| `--radius-sm` | 4px | 小元素（Tag、Badge） |
| `--radius-md` | 6px | 按钮、输入框 |
| `--radius-lg` | 8px | 卡片、弹窗 |
| `--radius-xl` | 12px | 大卡片 |
| `--radius-full` | 9999px | 圆形（头像、Pill） |

---

## 6. 阴影系统

```css
--shadow-sm:   <!-- FILL: 0 1px 2px rgba(0,0,0,0.1) -->;
--shadow-md:   <!-- FILL: 0 4px 6px rgba(0,0,0,0.1) -->;
--shadow-lg:   <!-- FILL: 0 10px 15px rgba(0,0,0,0.1) -->;
--shadow-xl:   <!-- FILL: 0 20px 25px rgba(0,0,0,0.1) -->;
--shadow-glow: <!-- FILL: 0 0 20px rgba(var(--color-primary-rgb), 0.3) -->;
```

---

## 7. 边框系统

| Token | 值 | 用途 |
|-------|-----|------|
| `--border-default` | 1px solid <!-- FILL --> | 默认边框 |
| `--border-strong` | 1px solid <!-- FILL --> | 强调边框 |
| `--border-subtle` | 1px solid <!-- FILL --> | 次要边框 |

---

## 8. 图标规范

| 属性 | 规范 |
|------|------|
| <!-- FILL: 图标库 --> | <!-- FILL: Lucide Icons --> |
| <!-- FILL: 尺寸 --> | <!-- FILL: 16px / 20px / 24px 三档 --> |
| <!-- FILL: 颜色 --> | <!-- FILL: 继承 currentColor --> |
| <!-- FILL: 线宽 --> | <!-- FILL: 1.5px / 2px --> |

---

## 9. 动效规范

| 属性 | 规范 |
|------|------|
| 时长 | <!-- FILL: 150ms（微交互）/ 200-300ms（过渡）/ 500ms+（入场） --> |
| 缓动 | <!-- FILL: ease-out（入场）/ ease-in（退场）/ cubic-bezier(0.4,0,0.2,1)（通用） --> |
| 触发 | <!-- FILL: hover / focus / scroll / mount --> |

---

## 10. 响应式断点

| Token | 值 | 设备 |
|-------|-----|------|
| `--breakpoint-sm` | <!-- FILL: 640px --> | 手机横屏 |
| `--breakpoint-md` | <!-- FILL: 768px --> | 平板竖屏 |
| `--breakpoint-lg` | <!-- FILL: 1024px --> | 平板横屏/小笔记本 |
| `--breakpoint-xl` | <!-- FILL: 1280px --> | 笔记本 |
| `--breakpoint-2xl` | <!-- FILL: 1536px --> | 桌面显示器 |

---

## 11. 可访问性

| 标准 | 要求 |
|------|------|
| 文字对比度 | ≥ 4.5:1（正文）/ ≥ 3:1（大字） |
| 焦点指示器 | 可见的 focus ring |
| 键盘导航 | 所有可交互元素支持 Tab |
| 屏幕阅读器 | 语义化 HTML + ARIA 标签 |

---

> **质量检查**: ✓ 色彩有深浅主题 ✓ 字体有梯度 ✓ 间距系统一致 ✓ 动效有统一规范
