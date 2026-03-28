# 组件库规范模板
<!-- FILL: 产品名称 -->
**产品**: <!-- FILL: Product Name -->
**文档版本**: 0.1.0
**最后更新**: <!-- FILL: YYYY-MM-DD -->
**技术栈**: React + TypeScript + Tailwind CSS

---

## 1. 组件分级（Atomic Design）

```
原子（Atom）        → Button, Input, Label, Icon, Badge, Avatar, Spinner
分子（Molecule）    → FormField, SearchBar, Dropdown, Toast, Tooltip, Tag
有机体（Organism）  → Modal, Sidebar, Navbar, Table, Card, DataGrid
模板（Template）    → PageLayout, AuthLayout, DashboardLayout
```

---

## 2. 通用组件 API 模板

> 为每个组件填写以下模板

### 组件名: <!-- FILL: ComponentName -->

**描述**: <!-- FILL: 一句话描述组件用途 -->

**文件位置**: `src/components/<!-- FILL: category/ComponentName.tsx -->`

#### Props

| Prop | 类型 | 必填 | 默认值 | 描述 |
|------|------|------|--------|------|
| <!-- FILL: propName --> | <!-- FILL: type --> | <!-- FILL: yes/no --> | <!-- FILL: default --> | <!-- FILL: desc --> |

#### Events（回调 Props）

| Prop | 类型 | 描述 |
|------|------|------|
| onClick | `(e: MouseEvent) => void` | 点击回调 |
| <!-- FILL --> | <!-- FILL --> | <!-- FILL --> |

#### 变体（Variants）

| Variant | 描述 |
|---------|------|
| default | 默认样式 |
| <!-- FILL --> | <!-- FILL --> |

#### 状态（States）

| 状态 | 触发条件 | 视觉表现 |
|------|---------|---------|
| default | 初始 | 正常样式 |
| hover | 鼠标悬停 | 背景色加深 |
| active / pressed | 鼠标按下 | scale(0.97) |
| focus | Tab 键聚焦 | 2px focus ring |
| disabled | `disabled` prop 为 true | 50% 透明度，pointer-events: none |
| loading | `loading` prop 为 true | 显示 Spinner |

#### 可访问性

- ARIA 角色：`<!-- FILL: role="button" -->`
- 键盘支持：<!-- FILL: Enter/Space 触发 onClick -->
- 颜色对比度：AA 级（4.5:1 以上）

#### 代码示例

```tsx
import { <!-- FILL: ComponentName --> } from '@/components/<!-- FILL: category/ComponentName -->';

// 基础用法
<<!-- FILL: ComponentName --> variant="primary" onClick={() => console.log('clicked')}>
  <!-- FILL: 按钮文字 -->
</<!-- FILL: ComponentName -->>

// 禁用状态
<<!-- FILL: ComponentName --> disabled>不可用</<!-- FILL: ComponentName -->>
```

#### 主题变量

```css
/* 在 globals.css 中自定义 */
--component-<!-- FILL: bg -->: #...;
--component-<!-- FILL: text -->: #...;
```

---

## 3. 已定义组件清单

### 3.1 Button（按钮）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| variant | `'primary' \| 'secondary' \| 'ghost' \| 'danger' \| 'link'` | `'primary'` | 样式变体 |
| size | `'sm' \| 'md' \| 'lg'` | `'md'` | 尺寸 |
| loading | boolean | false | 加载状态（显示 Spinner） |
| disabled | boolean | false | 禁用 |
| leftIcon | ReactNode | — | 左侧图标 |
| rightIcon | ReactNode | — | 右侧图标 |
| fullWidth | boolean | false | 宽度充满父容器 |
| onClick | `() => void` | — | 点击回调 |

变体尺寸：
- sm: `h-7 px-3 text-xs`
- md: `h-9 px-4 text-sm`
- lg: `h-11 px-6 text-base`

---

### 3.2 Input（输入框）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| type | string | `'text'` | 输入类型 |
| value | string | — | 受控值 |
| placeholder | string | — | 占位文字 |
| disabled | boolean | false | 禁用 |
| error | string | — | 错误提示文字 |
| prefix | ReactNode | — | 前缀内容 |
| suffix | ReactNode | — | 后缀内容 |
| clearable | boolean | false | 显示清除按钮 |
| onChange | `(v: string) => void` | — | 值变更回调 |

状态样式：
- 正常：`border border-border focus:border-brand`
- 错误：`border-error focus:ring-error/30`
- 禁用：`bg-muted opacity-50`

---

### 3.3 Select（下拉选择）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| options | `{value: string, label: string, disabled?: boolean}[]` | — | 选项列表 |
| value | string | — | 当前选中值 |
| placeholder | string | '请选择' | 占位文字 |
| multi | boolean | false | 多选模式 |
| searchable | boolean | false | 支持搜索 |
| onChange | `(v: string \| string[]) => void` | — | 变更回调 |

---

### 3.4 Modal（弹窗）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| isOpen | boolean | — | 控制显示/隐藏 |
| onClose | `() => void` | — | 关闭回调 |
| title | string | — | 标题 |
| size | `'sm' \| 'md' \| 'lg' \| 'xl' \| 'full'` | `'md'` | 尺寸 |
| closable | boolean | true | 是否显示关闭按钮 |
| maskClosable | boolean | true | 点击蒙层是否关闭 |
| footer | ReactNode | — | 底部内容区 |

尺寸映射：
- sm: `max-w-sm`（384px）
- md: `max-w-md`（448px）
- lg: `max-w-lg`（512px）
- xl: `max-w-2xl`（672px）
- full: 全屏

---

### 3.5 Toast（消息通知）

| Prop / 参数 | 类型 | 默认值 | 描述 |
|------------|------|--------|------|
| type | `'success' \| 'error' \| 'warning' \| 'info'` | `'info'` | 类型 |
| message | string | — | 消息内容 |
| duration | number | 3000 | 显示时长（ms），0=不自动关闭 |
| position | `'top' \| 'bottom' \| 'top-left' \| 'top-right' \| 'bottom-left' \| 'bottom-right'` | `'top-right'` | 位置 |

使用方式（Hook）：
```tsx
const { toast } = useToast();
toast('保存成功', 'success');
toast('操作失败，请重试', 'error', { duration: 5000 });
```

---

### 3.6 Dropdown（下拉菜单）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| trigger | ReactNode | — | 触发元素 |
| items | `DropdownItem[]` | — | 菜单项列表 |
| placement | `'bottom' \| 'bottom-start' \| 'bottom-end' \| 'top' \| 'right' \| 'left'` | `'bottom-start'` | 弹出位置 |
| offset | number | 8 | 与触发元素的间距 |

DropdownItem 类型：
```typescript
type DropdownItem = {
  key: string;
  label: string;
  icon?: ReactNode;
  danger?: boolean;           // 危险操作（红色）
  disabled?: boolean;
  onClick?: () => void;
} | { type: 'divider' };     // 分割线
```

---

### 3.7 Tabs（标签页）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| tabs | `{key: string, label: string, icon?: ReactNode, badge?: number}[]` | — | 标签列表 |
| activeKey | string | — | 当前激活 key |
| variant | `'line' \| 'pill' \| 'card'` | `'line'` | 样式变体 |
| onChange | `(key: string) => void` | — | 切换回调 |

---

### 3.8 Tooltip（工具提示）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| content | string \| ReactNode | — | 提示内容 |
| placement | string | `'top'` | 位置 |
| delay | number | 600 | 延迟显示（ms） |
| children | ReactNode | — | 触发元素 |

---

### 3.9 Card（卡片）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| variant | `'default' \| 'bordered' \| 'elevated'` | `'default'` | 样式 |
| padding | `'sm' \| 'md' \| 'lg' \| 'none'` | `'md'` | 内边距 |
| interactive | boolean | false | 是否可点击（hover 效果） |
| header | ReactNode | — | 卡片头部 |
| footer | ReactNode | — | 卡片底部 |

---

### 3.10 Table（表格）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| columns | `Column[]` | — | 列定义 |
| data | `Record<string, any>[]` | — | 数据源 |
| loading | boolean | false | 加载状态 |
| empty | ReactNode | — | 空状态内容 |
| pagination | PaginationConfig | — | 分页配置 |
| onRowClick | `(row: any) => void` | — | 行点击回调 |

Column 类型：
```typescript
type Column = {
  key: string;
  title: string;
  width?: number | string;
  align?: 'left' | 'center' | 'right';
  sortable?: boolean;
  render?: (value: any, row: any) => ReactNode;
};
```

---

### 3.11 Tag（标签）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| color | string | `'#5B7FFF'` | 标签颜色（HEX） |
| closable | boolean | false | 是否可关闭 |
| onClose | `() => void` | — | 关闭回调 |
| size | `'sm' \| 'md'` | `'md'` | 尺寸 |

---

### 3.12 Badge（徽标）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| count | number | — | 数量（>99 显示 99+） |
| dot | boolean | false | 圆点模式（不显示数字） |
| color | string | `'error'` | 颜色 |
| offset | [number, number] | [0, 0] | 偏移量 |

---

### 3.13 Avatar（头像）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| src | string | — | 图片 URL |
| name | string | — | 用户名（无图时显示首字母） |
| size | `'xs' \| 'sm' \| 'md' \| 'lg' \| 'xl'` | `'md'` | 尺寸 |
| shape | `'circle' \| 'square'` | `'circle'` | 形状 |

---

### 3.14 Switch（开关）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| checked | boolean | — | 受控状态 |
| disabled | boolean | false | 禁用 |
| label | string | — | 标签文字 |
| size | `'sm' \| 'md'` | `'md'` | 尺寸 |
| onChange | `(v: boolean) => void` | — | 变更回调 |

---

### 3.15 Skeleton（骨架屏）

| Prop | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| variant | `'text' \| 'rect' \| 'circle'` | `'text'` | 形状 |
| width | string \| number | `'100%'` | 宽度 |
| height | string \| number | `'1em'` | 高度 |
| lines | number | 1 | 文字行数（variant=text 时） |
| animated | boolean | true | 是否显示闪烁动画 |

---

## 4. 主题系统

所有组件通过 CSS 变量适配主题，无需修改组件代码：

```css
:root {
  --color-brand: #5B7FFF;
  --color-brand-hover: #4A6AE5;
  --color-bg: #FFFFFF;
  --color-surface: #F8F9FA;
  --color-border: #E9ECEF;
  --color-text: #1A1A2E;
  --color-text-secondary: #6B7280;
  --radius-base: 6px;
}

[data-theme="dark"] {
  --color-bg: #0D1117;
  --color-surface: #161B22;
  --color-border: #30363D;
  --color-text: #E6EDF3;
  --color-text-secondary: #8B949E;
}
```

---

## 5. Changelog

| 版本 | 日期 | 变更 |
|------|------|------|
| 0.1.0 | <!-- FILL: YYYY-MM-DD --> | 初始版本 |
