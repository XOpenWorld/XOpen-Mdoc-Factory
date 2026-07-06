# 🚀 DevOps 指南 (DevOps Guide)

> **📌 模板说明**: 本文档定义环境管理、CI/CD 流水线、监控告警和部署规范。AI 填写时请替换 `<!-- FILL -->` 部分。

---

## 🗂️ 元数据

| 字段 | 内容 |
|------|------|
| 📦 产品 | <!-- FILL: Product Name --> |
| 📅 最后更新 | <!-- FILL: YYYY-MM-DD --> |

---

## 🌍 1. 环境管理

| 环境 | 用途 | URL | 分支 |
|------|------|-----|------|
| development | 本地开发 | `http://localhost:3000` | feature/* / dev |
| staging | 预发布测试 | `https://staging.<!-- FILL: yourdomain.com -->` | main |
| production | 生产环境 | `https://<!-- FILL: yourdomain.com -->` | release/* / tag |

### 1.1 环境变量

```bash
# .env.development
VITE_API_BASE_URL=http://localhost:8080/v1
VITE_SUPABASE_URL=http://localhost:54321
VITE_SUPABASE_ANON_KEY=...（本地 Supabase 密钥）

# .env.staging
VITE_API_BASE_URL=https://staging-api.yourdomain.com/v1
VITE_SUPABASE_URL=...
VITE_SUPABASE_ANON_KEY=...

# .env.production
VITE_API_BASE_URL=https://api.yourdomain.com/v1
VITE_SUPABASE_URL=...
VITE_SUPABASE_ANON_KEY=...
```

**生产密钥**存放于 GitHub Secrets / Vault，绝不提交到仓库。

---

## ⚙️ 2. CI 流水线

### 2.1 GitHub Actions 配置

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main, 'release/**']
  pull_request:
    branches: [main]

jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run typecheck
      - run: npm run test:coverage
      - name: Upload coverage
        uses: codecov/codecov-action@v3

  build:
    needs: lint-and-test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v3
        with:
          name: dist
          path: dist/
```

### 2.2 CI 阶段说明

| 阶段 | 工具 | 失败处理 |
|------|------|---------|
| Lint | ESLint + Prettier | 阻断 PR 合并 |
| 类型检查 | TypeScript tsc | 阻断 PR 合并 |
| 单元测试 | Vitest | 阻断 PR 合并 |
| 构建 | Vite / Tauri | 阻断部署 |
| E2E（可选） | Playwright | 仅阻断生产部署 |
| 安全扫描 | npm audit / Snyk | 高危漏洞阻断 |

---

## 🚢 3. CD 策略

### 3.1 Staging 自动部署（每次 push main）

```yaml
# .github/workflows/deploy-staging.yml
name: Deploy Staging

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
        env:
          VITE_ENV: staging
          # 从 GitHub Secrets 注入
      - name: Deploy to Vercel (Staging)
        run: npx vercel --token ${{ secrets.VERCEL_TOKEN }} --env staging
```

### 3.2 生产发布（手动触发 + Tag）

```yaml
# .github/workflows/deploy-production.yml
name: Deploy Production

on:
  release:
    types: [published]

jobs:
  deploy-prod:
    runs-on: ubuntu-latest
    environment: production   # 需要人工审批
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
        env:
          VITE_ENV: production
      - name: Deploy to Vercel (Production)
        run: npx vercel --prod --token ${{ secrets.VERCEL_TOKEN }}
```

### 3.3 部署策略选择

| 策略 | 适用场景 | 工具 |
|------|---------|------|
| 蓝绿部署 | 关键服务，零停机 | Vercel / Cloudflare |
| 滚动更新 | 容器化服务 | Kubernetes / Docker Swarm |
| 金丝雀发布 | 新功能灰度验证 | Vercel Feature Flags / LaunchDarkly |

---

## 🐳 4. 容器化

### 4.1 Dockerfile（前端静态站点）

```dockerfile
# 构建阶段
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production=false
COPY . .
RUN npm run build

# 运行阶段
FROM nginx:1.25-alpine AS runner
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### 4.2 nginx.conf

```nginx
server {
  listen 80;
  root /usr/share/nginx/html;
  index index.html;

  # SPA 路由支持
  location / {
    try_files $uri $uri/ /index.html;
  }

  # 静态资源缓存
  location /assets/ {
    expires 1y;
    add_header Cache-Control "public, immutable";
  }

  # 安全 Headers
  add_header X-Frame-Options "DENY";
  add_header X-Content-Type-Options "nosniff";
}
```

### 4.3 docker-compose.yml（本地开发）

```yaml
version: '3.9'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:80"
    environment:
      - VITE_API_BASE_URL=http://api:8080/v1

  # 本地 Supabase（可选）
  supabase:
    image: supabase/supabase-db:latest
    ports:
      - "54321:5432"
    environment:
      POSTGRES_PASSWORD: postgres
```

---

## 🌐 5. 域名与 SSL

| 域名 | 用途 | SSL 来源 |
|------|------|---------|
| `<!-- FILL: yourdomain.com -->` | 主站/落地页 | Cloudflare / Let's Encrypt |
| `app.yourdomain.com` | 产品应用 | 同上 |
| `api.yourdomain.com` | API 服务 | 同上 |
| `cdn.yourdomain.com` | 静态资源 CDN | Cloudflare |

**HSTS 配置**：`max-age=31536000; includeSubDomains; preload`

---

## ⚡ 6. CDN 策略

| 资源类型 | 缓存策略 | CDN |
|---------|---------|-----|
| HTML | `no-store`（SPA 入口页不缓存） | Cloudflare |
| JS/CSS assets | `max-age=31536000, immutable`（Vite 带 hash） | Cloudflare |
| 图片 | `max-age=86400`（1 天） | Cloudflare Images |
| 用户上传文件 | 签名 URL + 1 小时 TTL | Supabase Storage / R2 |

---

## 📊 7. 日志收集

```
应用日志 → console（结构化 JSON）
        → Vercel Logs / CloudWatch
        → Loki（自托管） + Grafana

错误日志 → Sentry（前端 + 后端）
```

日志格式规范：
```json
{
  "level": "error",
  "message": "Database query failed",
  "timestamp": "2026-03-29T02:00:00Z",
  "userId": "usr_xxx",
  "requestId": "req_xxx",
  "stack": "..."
}
```

---

## 🚨 8. 错误监控（Sentry）

```typescript
// 前端接入
import * as Sentry from '@sentry/react';

Sentry.init({
  dsn: import.meta.env.VITE_SENTRY_DSN,
  environment: import.meta.env.MODE,
  tracesSampleRate: 0.1,         // 性能追踪采样率
  replaysSessionSampleRate: 0.1, // 会话录制采样率
  integrations: [
    Sentry.browserTracingIntegration(),
    Sentry.replayIntegration(),
  ],
  // 忽略非关键错误
  ignoreErrors: ['ResizeObserver loop limit exceeded'],
});
```

告警规则：
- 新错误首次出现 → 立即通知 Slack
- 错误频率 > 100次/小时 → 告警
- 性能 P95 > 3s → 告警

---

## 📈 9. 性能监控

| 指标 | 工具 | 目标 |
|------|------|------|
| FCP | Vercel Analytics / Lighthouse | < 1.5s |
| LCP | 同上 | < 2.5s |
| CLS | 同上 | < 0.1 |
| API P95 延迟 | Prometheus + Grafana | < 500ms |
| 错误率 | Sentry | < 0.5% |
| 可用性 | UptimeRobot | > 99.9% |

---

## 💾 10. 备份策略

| 类型 | 频率 | 保留期 | 存储 |
|------|------|--------|------|
| 数据库全量备份 | 每日 02:00 | 30 天 | 对象存储（加密） |
| 数据库增量（WAL） | 实时 | 7 天 | Supabase PITR |
| 用户文件 | 实时同步 | 永久 | R2 / OSS 多副本 |
| 代码仓库 | GitHub 自动 | 永久 | GitHub |

---

## 💰 11. 月度成本估算

<!-- FILL: 根据实际服务商填写，以下为估算参考 -->

| 服务 | 规格 | 月费（USD） |
|------|------|-----------|
| Vercel | Pro Plan | $20 |
| Supabase | Pro Plan | $25 |
| Cloudflare | Pro Plan | $20 |
| Sentry | Team Plan | $26 |
| AI API（DeepSeek/GLM-4） | ~10万 tokens/天 | ~$30 |
| 对象存储（R2/OSS） | 100GB | $5 |
| **合计** | — | **~$126/月** |

> 收入达到 $500/月 前，可用 Free Tier 将成本压到 < $30/月。

---

## 12. Changelog

| 版本 | 日期 | 变更 |
|------|------|------|
| 0.1.0 | <!-- FILL: YYYY-MM-DD --> | 初始版本 |
