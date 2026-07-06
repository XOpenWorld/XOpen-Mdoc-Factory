# 🧪 测试策略文档 (Testing Strategy)

> **📌 模板说明**: 本文档定义测试金字塔、工具选型、覆盖率和各层测试规范。AI 填写时请替换所有 `<!-- FILL -->` 占位符。

---

## 🗂️ 元数据

| 字段 | 内容 |
|------|------|
| 📦 产品 | <!-- FILL: Product Name --> |
| 📅 最后更新 | <!-- FILL: YYYY-MM-DD --> |

---

## 📊 1. 测试金字塔

```
           ┌─────┐
           │ E2E │  10%（少而精，核心流程）
          ┌┴─────┴┐
          │ 集成   │  20%（API + 组件集成）
         ┌┴───────┴┐
         │  单元   │  70%（Store / Utils / Services）
         └─────────┘
```

| 层级 | ⚖️ 比例 | 🎯 目标数量 | 🛠️ 工具 |
|------|---------|-----------|---------|
| 单元测试 | 70% | > 100 个 | Vitest + Testing Library |
| 集成测试 | 20% | > 30 个 | Vitest + MSW（Mock Service Worker） |
| E2E 测试 | 10% | > 15 个 | Playwright |

---

## 🔧 2. 工具选型

| 🛠️ 工具 | 📝 用途 | 🔖 版本 |
|---------|---------|--------|
| Vitest | 单元测试 + 集成测试 | ^2.0 |
| @testing-library/react | React 组件测试 | ^16.0 |
| @testing-library/user-event | 用户交互模拟 | ^14.0 |
| msw | API Mock（Service Worker）| ^2.0 |
| Playwright | E2E 测试 | ^1.40 |
| @vitest/coverage-v8 | 覆盖率报告 | ^2.0 |

---

## 🎯 3. 覆盖率目标

| 📁 模块 | 📏 行覆盖率 | 🌿 分支覆盖率 |
|--------|-----------|-------------|
| src/stores/ | ≥ 90% | ≥ 85% |
| src/services/ | ≥ 80% | ≥ 75% |
| src/lib/utils | ≥ 95% | ≥ 90% |
| src/components/ | ≥ 70% | ≥ 65% |
| 📊 整体 | ≥ 75% | ≥ 70% |

---

## 📝 4. 单元测试规范

### 4.1 📂 命名约定

```
src/
  stores/
    documentStore.ts
    documentStore.test.ts     ← 同目录，.test.ts 后缀
  lib/
    utils.ts
    utils.test.ts
  components/
    Button/
      Button.tsx
      Button.test.tsx
```

### 4.2 🔄 AAA 模式（Arrange-Act-Assert）

```typescript
describe('countWords', () => {
  it('should count Chinese characters correctly', () => {
    // Arrange（准备）
    const text = '你好世界';
    
    // Act（执行）
    const result = countWords(text);
    
    // Assert（断言）
    expect(result).toBe(4);
  });
});
```

### 4.3 🎭 Mock 策略

```typescript
// 外部 API → 用 vi.mock 或 MSW 拦截
vi.mock('../services/aiService', () => ({
  aiService: {
    streamComplete: vi.fn().mockResolvedValue({ content: 'mocked' })
  }
}));

// 时间 → 固定时间
vi.setSystemTime(new Date('2026-01-01'));

// localStorage → jsdom 自动支持，测试后 vi.clearAllMocks() 清理
```

### 4.4 📦 Store 测试模板

```typescript
import { beforeEach, describe, expect, it } from 'vitest';
import { useDocumentStore } from '../documentStore';

describe('documentStore', () => {
  beforeEach(() => {
    // 每个测试前重置 store 状态
    useDocumentStore.setState(useDocumentStore.getInitialState());
  });

  describe('createDocument', () => {
    it('should create a document with default title', () => {
      const { createDocument, documents } = useDocumentStore.getState();
      createDocument();
      expect(useDocumentStore.getState().documents).toHaveLength(1);
      expect(useDocumentStore.getState().documents[0].title).toBe('无标题');
    });
  });
});
```

---

## ⚛️ 5. 组件测试规范

```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Button } from './Button';

describe('Button', () => {
  it('renders children correctly', () => {
    render(<Button>点击我</Button>);
    expect(screen.getByText('点击我')).toBeInTheDocument();
  });

  it('calls onClick when clicked', async () => {
    const user = userEvent.setup();
    const handleClick = vi.fn();
    render(<Button onClick={handleClick}>点击</Button>);
    await user.click(screen.getByText('点击'));
    expect(handleClick).toHaveBeenCalledOnce();
  });

  it('is disabled when disabled prop is true', () => {
    render(<Button disabled>不可用</Button>);
    expect(screen.getByRole('button')).toBeDisabled();
  });
});
```

---

## 🌐 6. E2E 测试场景

| 🎬 场景 | 📋 步骤 | ✅ 预期结果 |
|---------|--------|-----------|
| 注册新用户 | 填写邮箱/密码 → 提交 | 跳转到主界面 |
| 创建文档 | 点击新建 → 输入内容 → Ctrl+S | 文档出现在侧边栏 |
| 编辑并预览 | 输入 Markdown → 切换预览模式 | 预览正确渲染 |
| 文件夹操作 | 新建文件夹 → 拖拽文档 | 文档移入文件夹 |
| 搜索文档 | 输入关键词 | 搜索结果高亮匹配 |
| 导出 PDF | 点击导出 → 选择 PDF | 打印对话框弹出 |
| 分享文档 | 点击分享 → 复制链接 | 链接可访问分享页 |
| AI 续写 | 选中文本 → 点击续写 | 流式输出内容 |
| 标签管理 | 添加标签 → 按标签筛选 | 文档列表按标签过滤 |
| 登出 | 点击登出 | 跳转到登录页 |

### ⚙️ E2E 配置

```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  fullyParallel: true,
  retries: process.env.CI ? 2 : 0,
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    { name: 'chromium', use: { browserName: 'chromium' } },
    { name: 'firefox', use: { browserName: 'firefox' } },
    { name: 'mobile', use: { ...devices['Pixel 7'] } },
  ],
  webServer: {
    command: 'npm run dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
});
```

---

## ⚡ 7. 性能测试

| 🎬 场景 | 🛠️ 工具 | 🎯 阈值 |
|---------|--------|--------|
| 10000 字文档加载渲染 | Playwright + performance API | < 500ms |
| 实时预览延迟 | Vitest benchmark | < 100ms |
| Store 写入性能 | Vitest benchmark | < 10ms |
| 首屏加载 | Lighthouse CI | LCP < 2.5s |

```typescript
// Vitest benchmark 示例
import { bench, describe } from 'vitest';

describe('markdown render performance', () => {
  bench('render 10000 chars', () => {
    renderMarkdown(longMarkdownText);
  });
});
```

---

## 🔄 8. CI 集成

```yaml
# .github/workflows/ci.yml（测试部分）
- name: Run unit tests with coverage
  run: npm run test:coverage
  
- name: Check coverage thresholds
  run: npm run test:coverage -- --coverage.thresholds.lines=75

- name: Run E2E tests (on PR to main)
  if: github.base_ref == 'main'
  run: npx playwright test
  env:
    BASE_URL: http://localhost:3000
```

---

## 📊 9. 测试报告

- 单元测试覆盖率报告：上传至 Codecov，PR 中显示覆盖率变化
- E2E 报告：Playwright HTML Report，失败时上传截图/视频到 CI Artifacts
- 性能基准：每次发布后记录到 `performance-baseline.json`，对比回归

---

## 📋 10. Changelog

| 🔖 版本 | 📅 日期 | 📝 变更 |
|---------|--------|---------|
| 0.1.0 | <!-- FILL: YYYY-MM-DD --> | 初始版本 |

---

> **✅ 质量检查**: ✓ 测试金字塔比例合理 ✓ 覆盖率目标明确 ✓ 各层测试有规范模板 ✓ CI 集成完整
