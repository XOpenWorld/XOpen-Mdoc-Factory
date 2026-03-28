# 贡献指南模板
<!-- FILL: 产品名称 -->
**产品**: <!-- FILL: Product Name -->
**文档版本**: 0.1.0
**最后更新**: <!-- FILL: YYYY-MM-DD -->

---

# 贡献指南 Contributing Guide

感谢你对 **<!-- FILL: 产品名称 -->** 的贡献！无论是 Bug 修复、功能建议还是文档改进，我们都非常欢迎。

---

## 1. 环境要求

| 工具 | 最低版本 | 推荐版本 |
|------|---------|---------|
| Node.js | 18.x | 20.x LTS |
| npm | 9.x | 10.x |
| Git | 2.x | 最新 |
| <!-- FILL: 其他依赖 --> | — | — |

> Windows 用户建议使用 WSL2 或 Git Bash 运行命令。

---

## 2. 快速开始

```bash
# 1. Fork 仓库并 Clone
git clone https://github.com/<!-- FILL: yourorg/yourrepo -->.git
cd <!-- FILL: yourrepo -->

# 2. 安装依赖
npm install

# 3. 复制环境变量
cp .env.example .env.local
# 编辑 .env.local，填写必要配置

# 4. 启动开发服务器
npm run dev
# 访问 http://localhost:3000
```

---

## 3. 开发命令

| 命令 | 说明 |
|------|------|
| `npm run dev` | 启动开发服务器（热重载） |
| `npm run build` | 生产构建 |
| `npm run preview` | 预览生产构建结果 |
| `npm run test` | 运行单元测试 |
| `npm run test:coverage` | 运行测试并生成覆盖率报告 |
| `npm run test:e2e` | 运行 E2E 测试（需先 `npm run dev`） |
| `npm run lint` | ESLint 代码检查 |
| `npm run lint:fix` | 自动修复 Lint 问题 |
| `npm run typecheck` | TypeScript 类型检查 |
| `npm run format` | Prettier 格式化 |

---

## 4. 项目结构

```
src/
  assets/          ← 静态资源（图片、字体）
  components/      ← UI 组件
    common/        ← 通用组件（Button、Modal、Toast...）
    editor/        ← 编辑器相关组件
    sidebar/       ← 侧边栏组件
  hooks/           ← 自定义 React Hooks
  i18n/            ← 国际化
    locales/
  lib/             ← 工具函数 + 纯逻辑
  pages/           ← 路由页面组件
  services/        ← 外部 API 调用（AI、分享、云同步）
  stores/          ← Zustand 状态管理
  styles/          ← 全局 CSS
  types/           ← TypeScript 类型定义
tests/
  unit/            ← 单元测试
  e2e/             ← Playwright E2E 测试
```

---

## 5. 代码规范

### 5.1 ESLint + Prettier

提交前自动检查，CI 中强制通过。主要规则：
- TypeScript 严格模式（`strict: true`）
- 禁止 `any` 类型（除特殊场景需注释说明）
- 组件 Props 必须有类型定义
- `console.log` 禁止在生产代码中（仅允许 `console.error` 和 `console.warn`）

### 5.2 组件规范

```typescript
// ✅ 正确：函数式组件 + 明确的 Props 类型
interface ButtonProps {
  variant?: 'primary' | 'secondary';
  onClick?: () => void;
  children: React.ReactNode;
}

export const Button: React.FC<ButtonProps> = ({ variant = 'primary', onClick, children }) => {
  return <button className={styles[variant]} onClick={onClick}>{children}</button>;
};

// ❌ 错误：any 类型、无类型定义
export const Button = (props: any) => { ... };
```

### 5.3 文件命名

| 类型 | 命名规范 | 示例 |
|------|---------|------|
| 组件文件 | PascalCase | `Button.tsx`, `DocumentCard.tsx` |
| Hook 文件 | camelCase，`use` 前缀 | `useTheme.ts` |
| Store 文件 | camelCase，`Store` 后缀 | `documentStore.ts` |
| 工具函数 | camelCase | `formatDate.ts` |
| 类型定义 | camelCase | `document.types.ts` |
| 测试文件 | 同被测文件，`.test.` 后缀 | `Button.test.tsx` |

---

## 6. Git 分支策略

```
main          ← 主分支，保持可发布状态
  └── release/v1.x.x    ← 发布分支
      └── hotfix/xxx     ← 紧急修复

feature/xxx   ← 功能开发（从 main 分出）
fix/xxx       ← Bug 修复（从 main 分出）
docs/xxx      ← 文档更新
chore/xxx     ← 构建/配置修改
```

**规则**：
- 禁止直接 push 到 `main`，必须通过 PR
- 每个 PR 对应一个明确的功能/修复
- PR 必须通过 CI 才能合并

---

## 7. Commit 规范

遵循 **Conventional Commits** 规范：

```
<type>(<scope>): <subject>

[body]

[footer]
```

**type 类型**：

| type | 含义 |
|------|------|
| `feat` | 新功能 |
| `fix` | Bug 修复 |
| `docs` | 文档更新 |
| `style` | 代码格式（不影响逻辑） |
| `refactor` | 重构（不含新功能/修复） |
| `test` | 添加或修改测试 |
| `chore` | 构建/配置/依赖更新 |
| `perf` | 性能优化 |
| `ci` | CI 配置修改 |

**示例**：

```
feat(editor): add Mermaid diagram support

Add mermaid.js integration to render flowcharts,
sequence diagrams and Gantt charts from code blocks.

Closes #123
```

```
fix(sync): handle token expiration correctly

Previously, expired tokens caused an infinite loop.
Now we refresh the token and retry the request.

Fixes #456
```

---

## 8. PR 流程

### 8.1 提交 PR 前检查

- [ ] 代码在本地运行正常
- [ ] `npm run lint` 无报错
- [ ] `npm run typecheck` 无报错
- [ ] `npm run test` 全部通过
- [ ] 新功能已添加对应测试
- [ ] 文档/注释已更新

### 8.2 PR 描述模板

```markdown
## 变更说明

<!-- 简洁描述本次 PR 做了什么 -->

## 变更类型

- [ ] 新功能（feat）
- [ ] Bug 修复（fix）
- [ ] 重构（refactor）
- [ ] 文档更新（docs）
- [ ] 其他：___

## 关联 Issue

Closes #<!-- FILL: issue 编号 -->

## 测试说明

<!-- 如何测试本次变更 -->

## 截图（可选）

<!-- 如果是 UI 变更，提供截图 -->
```

---

## 9. Issue 模板

### 9.1 Bug Report

```markdown
**描述**：简洁描述 Bug

**复现步骤**：
1. 进入 '...'
2. 点击 '...'
3. 发现 '...'

**期望行为**：应该发生什么

**实际行为**：实际发生了什么

**截图**：（可选）

**环境**：
- 操作系统：
- 浏览器版本：
- 应用版本：
```

### 9.2 Feature Request

```markdown
**功能描述**：简洁描述你希望的功能

**使用场景**：在什么情况下你需要这个功能？

**期望行为**：你希望它如何工作？

**替代方案**：是否考虑过其他解决方法？

**优先级**：高 / 中 / 低
```

---

## 10. 行为准则

请参阅 [Contributor Covenant Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/)。

简要原则：
- 尊重所有参与者，不论技术水平
- 接受建设性批评
- 专注于对项目最好的决策
- 不接受骚扰、歧视性言论

---

## 11. License

本项目遵循 <!-- FILL: MIT / Apache-2.0 / GPL-3.0 --> 许可证。提交 PR 即表示你同意将你的贡献纳入本项目的许可证范围。

---

## 12. 联系方式

- GitHub Issues：[项目 Issues 页](https://github.com/<!-- FILL: yourorg/yourrepo -->/issues)
- 邮件：`<!-- FILL: dev@yourdomain.com -->`
- <!-- FILL: Discord / Slack 社区链接（可选） -->
