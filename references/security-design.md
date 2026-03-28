# 安全设计文档模板
<!-- FILL: 产品名称 -->
**产品**: <!-- FILL: Product Name -->
**文档版本**: 0.1.0
**最后更新**: <!-- FILL: YYYY-MM-DD -->
**状态**: Draft / Review / Final

---

## 1. 安全目标

| 目标 | 描述 |
|------|------|
| 机密性 | 用户数据只有授权方可访问 |
| 完整性 | 数据在传输和存储中不被篡改 |
| 可用性 | 系统可承受合理负载，防范 DDoS |
| 可审计性 | 关键操作有日志可追溯 |
| 合规性 | 符合 GDPR、中国个人信息保护法 |

---

## 2. 威胁模型（STRIDE）

| 威胁类别 | 全称 | 典型场景 | 防护措施 |
|---------|------|---------|---------|
| Spoofing | 身份仿冒 | 伪造 JWT / OAuth Token | JWT 签名验证，短有效期 |
| Tampering | 数据篡改 | 修改请求体，伪造操作 | 服务端验证，RLS |
| Repudiation | 抵赖 | 用户否认操作 | 操作日志（activity_log） |
| Information Disclosure | 信息泄露 | SQL 注入、XSS、敏感字段暴露 | 参数化查询、CSP、字段过滤 |
| Denial of Service | 拒绝服务 | 高并发请求、大文件上传 | 速率限制、文件大小限制 |
| Elevation of Privilege | 权限提升 | 越权访问其他用户数据 | RBAC + RLS，最小权限原则 |

---

## 3. 认证与授权

### 3.1 密码策略

- 最小长度：8 位
- 必须包含：大写字母 + 数字（或特殊字符）
- 密码 Hash：bcrypt（cost factor = 12）或 Argon2id
- 不允许使用常见弱密码（rockyou 词单）
- 连续失败 5 次后锁定账号 15 分钟

### 3.2 JWT 配置

```
Access Token：
  算法: RS256（非对称签名）
  有效期: 15 分钟
  Claims: sub (userId), email, plan, iat, exp

Refresh Token：
  存储位置: HttpOnly + Secure Cookie
  有效期: 30 天（滑动续期）
  绑定设备: user_agent + ip_fingerprint（可选）
```

### 3.3 OAuth2 社交登录

```
支持: Google / GitHub
流程:
  1. 前端重定向到授权端点
  2. 用户授权后携带 code 回调
  3. 服务端用 code 换取 access_token
  4. 通过 access_token 获取 userinfo
  5. 查找或创建本地用户，颁发 JWT
安全要求:
  - state 参数防 CSRF
  - PKCE 防授权码截获（公共客户端必须）
  - redirect_uri 白名单校验
```

### 3.4 RBAC 角色权限

| 角色 | 描述 | 权限范围 |
|------|------|---------|
| anonymous | 未登录访客 | 只能查看公开分享链接 |
| user_free | 免费用户 | 管理自己的文档/文件夹/标签 |
| user_pro | 付费用户 | 同上 + 扩展存储 + AI 配额 |
| admin | 系统管理员 | 全量访问（仅内部使用） |

---

## 4. 数据加密

### 4.1 传输层

- 全站强制 HTTPS（TLS 1.2+）
- HTTP Strict Transport Security（HSTS）：`max-age=31536000; includeSubDomains`
- WebSocket 使用 WSS
- API 证书：Let's Encrypt / Cloudflare

### 4.2 存储加密

| 数据类型 | 加密方案 | 密钥管理 |
|---------|---------|---------|
| 用户密码 | bcrypt / Argon2id Hash | 无需解密 |
| 分享密码 | bcrypt Hash | 无需解密 |
| 数据库静态数据 | AES-256（磁盘加密） | 云服务托管密钥 |
| 文件附件 | AES-256-GCM | 每文件唯一密钥，存储于密钥管理服务 |
| 敏感配置（AI Key） | 应用层加密 + 环境变量 | 禁止明文存储 |

### 4.3 端到端加密（可选高级功能）

<!-- OPTIONAL: 如果产品支持端到端加密 -->
- 使用 libsodium（X25519 + XSalsa20-Poly1305）
- 密钥在客户端生成，服务端不持有
- 私钥加密存储于本地（密码派生 key 加密私钥）

---

## 5. OWASP Top 10 防护清单

| # | 威胁 | 防护措施 | 实现方式 |
|---|------|---------|---------|
| A01 | 访问控制失效 | RBAC + RLS | PostgreSQL RLS + 服务端权限校验 |
| A02 | 加密失败 | 全程 TLS + 数据加密 | HSTS + 存储加密 |
| A03 | 注入攻击 | 参数化查询 | Supabase SDK / Prisma ORM |
| A04 | 不安全设计 | 威胁建模 | 本文档 |
| A05 | 安全配置错误 | 安全基线 | 生产环境 Checklist |
| A06 | 过时组件 | 依赖版本管理 | Dependabot / npm audit |
| A07 | 认证失败 | 强密码 + JWT | bcrypt + RS256 JWT |
| A08 | 软件数据完整性 | 签名验证 | GitHub Actions + 代码签名 |
| A09 | 日志不足 | 全量操作日志 | activity_log 表 |
| A10 | SSRF | 白名单 URL 验证 | 后端 URL 白名单校验 |

---

## 6. 前端安全

### 6.1 Content Security Policy（CSP）

```
Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'nonce-{RANDOM}';
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https://cdn.yourdomain.com;
  font-src 'self';
  connect-src 'self' https://api.yourdomain.com wss://api.yourdomain.com;
  frame-ancestors 'none';
  base-uri 'self';
  form-action 'self';
```

### 6.2 其他安全 Headers

```
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

### 6.3 XSS 防护

- Markdown 渲染使用 DOMPurify 净化 HTML 输出
- 禁止 `dangerouslySetInnerHTML`（必须用时需 sanitize）
- 用户输入不直接拼接 SQL / 命令

---

## 7. 文件上传安全

| 约束 | 值 |
|------|----|
| 允许类型 | `.md`, `.txt`, `.png`, `.jpg`, `.webp`, `.pdf` |
| 最大文件大小 | 10MB（单文件），100MB（总存储，免费版） |
| 文件名净化 | 移除路径分隔符、特殊字符，仅保留字母/数字/.-_ |
| 存储路径 | `{userId}/{random_uuid}.{ext}`（防遍历） |
| CDN 访问控制 | 签名 URL（有效期 1 小时） |

---

## 8. AI 安全

### 8.1 Prompt Injection 防护

- 用户输入与系统 Prompt 严格分离
- 系统 Prompt 不包含敏感信息
- 输入字符数限制（默认 4000 tokens）
- 异常 Prompt 模式检测（含 `ignore previous instructions` 等关键词）

### 8.2 输出过滤

- 过滤模型输出中的 PII（手机号、身份证、银行卡正则匹配）
- 过滤有害内容（接入内容安全 API，如腾讯云内容安全）

### 8.3 AI API 密钥安全

- 密钥只存储于服务端环境变量
- 前端不可见 AI 密钥
- 用量监控 + 超额告警
- 按用户/计划设置 Token 配额

---

## 9. API 安全

### 9.1 速率限制

参见 API 接口文档 §7

### 9.2 CORS 配置

```javascript
// 允许来源白名单
const allowedOrigins = [
  'https://app.yourdomain.com',
  'https://yourdomain.com',
  // 开发环境
  process.env.NODE_ENV === 'development' && 'http://localhost:3000',
].filter(Boolean);
```

### 9.3 CSRF 防护

- API 纯 JSON 接口（不接受表单提交），配合 SameSite Cookie 天然防 CSRF
- 需要额外保护的接口：添加 `X-CSRF-Token` Header

---

## 10. 合规

### 10.1 GDPR（欧盟用户）

| 要求 | 实现方案 |
|------|---------|
| 数据最小化 | 只收集必要字段 |
| 目的限制 | 明确告知数据用途 |
| 用户访问权 | 提供数据导出功能（JSON/ZIP） |
| 被遗忘权 | 账号注销后 30 天内彻底删除 |
| 数据可携性 | 支持导出所有文档 |
| 同意管理 | Cookie 同意横幅 |

### 10.2 中国个人信息保护法（PIPL）

- 隐私政策：用中文清晰说明收集范围和用途
- 第三方 SDK 声明：列出所有第三方服务及其数据访问权限
- 数据本地化：中国用户数据存储于境内（如使用腾讯云/阿里云国内区域）
- 未成年人保护：注册时年龄验证，14 岁以下需监护人授权

### 10.3 AI 生成内容免责声明

- 页面明确标注「AI 辅助生成」
- 不对 AI 输出内容的准确性负责的免责条款
- 遵守《互联网信息服务深度合成管理规定》

---

## 11. 密钥管理

| 密钥类型 | 存储方式 | 轮换周期 | 谁可访问 |
|---------|---------|---------|---------|
| JWT 签名私钥 | 环境变量 / Secrets Manager | 90 天 | 后端服务 |
| 数据库连接串 | 环境变量 | 变更即换 | 后端服务 |
| AI API Key | 环境变量 + 密钥管理服务 | 30 天 | 后端服务 |
| OAuth Client Secret | 环境变量 | 180 天 | 后端服务 |
| 存储 Bucket 密钥 | 环境变量 | 90 天 | 后端服务 |

**禁止**：
- ❌ 密钥硬编码到代码
- ❌ 密钥提交到 Git 仓库
- ❌ 生产密钥用于开发环境

---

## 12. 安全事件响应

### 12.1 响应流程

```
发现安全事件
    ↓
立即评估影响范围（5分钟内）
    ↓
隔离受影响系统（如需要）
    ↓
通知核心团队
    ↓
取证 & 分析根因
    ↓
修复 & 部署补丁
    ↓
通知受影响用户（如数据泄露）
    ↓
事后复盘 & 更新安全策略
```

### 12.2 数据泄露通知

- GDPR 要求：72 小时内通知监管机构（DPA），情节严重需通知用户
- PIPL 要求：立即采取补救措施，按需通知监管机构

### 12.3 安全漏洞披露

- 漏洞报告邮箱：`security@<!-- FILL: yourdomain.com -->`
- 承诺 48 小时响应
- 负责任披露策略：给予 90 天修复窗口期

---

## 13. 安全审计清单

每季度执行：
- [ ] 依赖库漏洞扫描（`npm audit`）
- [ ] 第三方服务权限审查
- [ ] 访问日志异常分析
- [ ] 密钥轮换

每半年执行：
- [ ] 渗透测试（外部安全团队）
- [ ] 代码安全审查
- [ ] 合规文档更新

发版前执行：
- [ ] OWASP Top 10 Checklist 过一遍
- [ ] 新功能威胁模型评估
- [ ] 敏感字段是否暴露检查

---

## 14. Changelog

| 版本 | 日期 | 变更 |
|------|------|------|
| 0.1.0 | <!-- FILL: YYYY-MM-DD --> | 初始版本 |
