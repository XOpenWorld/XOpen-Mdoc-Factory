# 国际化（i18n）规范模板
<!-- FILL: 产品名称 -->
**产品**: <!-- FILL: Product Name -->
**文档版本**: 0.1.0
**最后更新**: <!-- FILL: YYYY-MM-DD -->

---

## 1. 语言支持优先级

| 优先级 | 语言代码 | 语言名称 | 状态 |
|--------|---------|---------|------|
| P0 | `zh-CN` | 简体中文 | ✅ 主语言 |
| P0 | `en-US` | 英语（美国） | ✅ 已完成 |
| P1 | `zh-TW` | 繁体中文 | 🔲 待翻译 |
| P1 | `ja` | 日语 | 🔲 待翻译 |
| P2 | `ko` | 韩语 | 🔲 规划中 |
| P2 | `de` | 德语 | 🔲 规划中 |
| P2 | `fr` | 法语 | 🔲 规划中 |
| P3 | `ar` | 阿拉伯语（RTL） | 🔲 远期规划 |

浏览器语言自动检测优先级：
1. `localStorage.getItem('language')` 用户偏好
2. `navigator.language`
3. 默认 `zh-CN`

---

## 2. 翻译文件结构

```
src/
  i18n/
    index.ts             ← i18n 初始化 + useT() hook
    locales/
      zh-CN/
        common.json       ← 通用文字（按钮、状态、提示）
        editor.json       ← 编辑器相关
        sidebar.json      ← 侧边栏
        settings.json     ← 设置面板
        ai.json           ← AI 功能
        share.json        ← 分享功能
        errors.json       ← 错误信息
      en-US/
        common.json
        ... （同结构）
      zh-TW/
        ... （同结构）
```

### 2.1 JSON 格式规范

```json
{
  "button": {
    "save": "保存",
    "cancel": "取消",
    "confirm": "确认",
    "delete": "删除",
    "create": "新建",
    "import": "导入",
    "export": "导出"
  },
  "status": {
    "saved": "已保存",
    "saving": "保存中...",
    "unsaved": "未保存",
    "offline": "离线"
  },
  "error": {
    "network": "网络连接失败，请稍后重试",
    "unknown": "出现未知错误"
  },
  "count": {
    "documents": "{{count}} 篇文档",
    "words": "{{count}} 字"
  }
}
```

### 2.2 带变量插值

```json
{
  "greeting": "你好，{{name}}！",
  "docCount": "共 {{count}} 篇文档",
  "lastEdited": "最后编辑于 {{time}}"
}
```

使用：
```typescript
t('greeting', { name: '张三' })  // → "你好，张三！"
t('docCount', { count: 42 })    // → "共 42 篇文档"
```

### 2.3 复数规则（英语等需要）

```json
{
  "en-US": {
    "document_count": {
      "one": "{{count}} document",
      "other": "{{count}} documents"
    }
  }
}
```

---

## 3. i18n 初始化示例

```typescript
// src/i18n/index.ts
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';

// 按需动态加载 locale
const loadLocale = async (lang: string) => {
  const [common, editor, sidebar] = await Promise.all([
    import(`./locales/${lang}/common.json`),
    import(`./locales/${lang}/editor.json`),
    import(`./locales/${lang}/sidebar.json`),
  ]);
  return { common: common.default, editor: editor.default, sidebar: sidebar.default };
};

const defaultLang = localStorage.getItem('language') 
  || navigator.language.split('-')[0] === 'zh' ? 'zh-CN' : 'en-US';

const resources = {
  'zh-CN': await loadLocale('zh-CN'),
  'en-US': await loadLocale('en-US'),
};

i18n.use(initReactI18next).init({
  resources,
  lng: defaultLang,
  fallbackLng: 'zh-CN',
  interpolation: { escapeValue: false },
  ns: ['common', 'editor', 'sidebar', 'settings', 'ai', 'share', 'errors'],
  defaultNS: 'common',
});

export default i18n;
export const useT = () => useTranslation();
```

---

## 4. 日期 / 时间 / 货币格式

| 格式类型 | zh-CN | en-US |
|---------|-------|-------|
| 短日期 | 2026年3月29日 | Mar 29, 2026 |
| 相对时间 | 3分钟前 | 3 minutes ago |
| 时间 | 下午 2:30 | 2:30 PM |
| 货币 | ¥99.00 | $9.99 |
| 数字千分位 | 1,000,000 | 1,000,000 |

```typescript
// 使用 Intl API 格式化（无需额外依赖）
const formatDate = (date: Date, locale: string) =>
  new Intl.DateTimeFormat(locale, { dateStyle: 'medium' }).format(date);

const formatCurrency = (amount: number, locale: string, currency: string) =>
  new Intl.NumberFormat(locale, { style: 'currency', currency }).format(amount);
```

---

## 5. RTL 适配预案

<!-- OPTIONAL: 如果计划支持阿拉伯语/希伯来语等 RTL 语言 -->

```html
<!-- 动态设置 dir 属性 -->
<html dir="rtl" lang="ar">
```

```css
/* 使用逻辑属性代替物理属性 */
margin-inline-start: 16px;  /* 代替 margin-left */
padding-inline-end: 8px;    /* 代替 padding-right */
```

Tailwind CSS 开启 RTL 支持：
```javascript
// tailwind.config.js
module.exports = {
  plugins: [require('tailwindcss-rtl')],
};
```

---

## 6. AI 内容多语言策略

- AI 系统 Prompt 检测用户语言，用对应语言回复
- 所有 AI 功能入口（续写/摘要/翻译/对话）的 Prompt 模板均有多语言版本
- 翻译功能：支持目标语言选择（检测原文语言 → 用户选择目标语言）

```typescript
const buildAIPrompt = (action: string, content: string, lang: string) => {
  const prompts: Record<string, Record<string, string>> = {
    'zh-CN': {
      continue: `请继续写以下内容，保持原有风格：\n\n${content}`,
      summarize: `请用 3-5 句话总结以下内容：\n\n${content}`,
    },
    'en-US': {
      continue: `Continue writing the following content, maintaining the same style:\n\n${content}`,
      summarize: `Summarize the following in 3-5 sentences:\n\n${content}`,
    },
  };
  return prompts[lang]?.[action] ?? prompts['en-US'][action];
};
```

---

## 7. 翻译管理流程

| 阶段 | 工具 | 负责人 |
|------|------|--------|
| 翻译文件管理 | Git 版本控制 | 开发 |
| 翻译协作（可选） | Crowdin / Weblate | 翻译 |
| 术语审核 | 术语表（见第 8 节） | PM |
| 覆盖率检查 | i18n-ally VSCode 插件 | 开发 |
| 上线前验证 | 截图对比 + 自动化测试 | QA |

新增 key 流程：
1. 开发先在 `zh-CN` 文件中添加 key
2. 同时添加 `en-US` 翻译
3. PR 合并时自动检测是否有缺失 key（通过 CI lint 脚本）

---

## 8. 术语表（标准翻译对照）

> 产品功能名称必须使用以下标准翻译，禁止随意翻译。

| 功能名称（源语言） | 简体中文 | English |
|-----------------|---------|---------|
| <!-- FILL: 产品名 --> | <!-- FILL: 中文 --> | <!-- FILL: English --> |
| Document | 文档 | Document |
| Folder | 文件夹 | Folder |
| Tag | 标签 | Tag |
| Share | 分享 | Share |
| Export | 导出 | Export |
| AI Assist | AI 辅助 | AI Assist |
| Template | 模板 | Template |
| Preview | 预览 | Preview |
| Sync | 同步 | Sync |
| Starred | 已收藏 | Starred |
| Trash | 回收站 | Trash |

---

## 9. 翻译覆盖率目标

| 语言 | 覆盖率目标 | 当前状态 |
|------|----------|---------|
| zh-CN | 100% | ✅ |
| en-US | 100% | ✅ |
| zh-TW | ≥ 90% | 🔲 待完成 |
| ja | ≥ 80% | 🔲 待完成 |

---

## 10. Changelog

| 版本 | 日期 | 变更 |
|------|------|------|
| 0.1.0 | <!-- FILL: YYYY-MM-DD --> | 初始版本 |
