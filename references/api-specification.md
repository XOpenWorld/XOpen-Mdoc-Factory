# API 接口文档模板
<!-- FILL: 产品名称 -->
**产品**: <!-- FILL: Product Name -->
**API 版本**: v1.0
**文档版本**: 0.1.0
**最后更新**: <!-- FILL: YYYY-MM-DD -->
**状态**: Draft / Review / Final

---

## 1. 通用规范

### 1.1 Base URL

```
开发环境: https://dev-api.<!-- FILL: yourdomain.com -->/v1
生产环境: https://api.<!-- FILL: yourdomain.com -->/v1
```

### 1.2 认证方式

| 方式 | 适用场景 | Header / 参数 |
|------|---------|--------------|
| Bearer JWT | 用户认证接口 | `Authorization: Bearer <token>` |
| API Key | 服务器端调用 | `X-API-Key: <key>` |
| 无认证 | 公开接口 | — |

JWT 有效期：Access Token `15min`，Refresh Token `30d`

### 1.3 请求格式

```
Content-Type: application/json
Accept: application/json
Accept-Language: zh-CN, en-US
```

### 1.4 响应格式

**成功响应**:
```json
{
  "code": 0,
  "message": "success",
  "data": { ... },
  "timestamp": 1711699200000
}
```

**错误响应**:
```json
{
  "code": 40001,
  "message": "Unauthorized",
  "details": "Token expired",
  "timestamp": 1711699200000,
  "requestId": "req_abc123"
}
```

### 1.5 错误码体系

| HTTP 状态 | 业务错误码范围 | 含义 |
|-----------|-------------|------|
| 200 | 0 | 成功 |
| 400 | 40000–40099 | 请求参数错误 |
| 401 | 40100–40199 | 未认证 |
| 403 | 40300–40399 | 无权限 |
| 404 | 40400–40499 | 资源不存在 |
| 409 | 40900–40999 | 资源冲突 |
| 422 | 42200–42299 | 数据验证失败 |
| 429 | 42900–42999 | 频率限制 |
| 500 | 50000–59999 | 服务端错误 |

常用错误码：

| 错误码 | 含义 |
|--------|------|
| 40001 | Token 过期 |
| 40002 | Token 无效 |
| 40101 | 用户名或密码错误 |
| 40301 | 无访问权限 |
| 40401 | 文档不存在 |
| 42201 | 参数格式错误 |
| 42901 | 请求过于频繁 |

### 1.6 分页参数

```
GET /resources?page=1&pageSize=20&sort=created_at&order=desc
```

响应体中包含：
```json
{
  "data": [ ... ],
  "pagination": {
    "page": 1,
    "pageSize": 20,
    "total": 145,
    "totalPages": 8
  }
}
```

### 1.7 版本管理策略

- URL 路径版本号：`/v1/`, `/v2/`
- 重大变更时升级主版本
- 旧版本保留 12 个月过渡期
- `Deprecation` 响应头通知即将弃用的接口

---

## 2. 认证 API

### 2.1 注册

```
POST /auth/register
```

请求体：
```json
{
  "email": "user@example.com",
  "password": "Str0ng!Pass",
  "name": "张三",
  "inviteCode": "OPTIONAL"
}
```

响应：
```json
{
  "code": 0,
  "data": {
    "userId": "usr_abc123",
    "email": "user@example.com",
    "name": "张三",
    "accessToken": "eyJ...",
    "refreshToken": "eyJ...",
    "expiresAt": 1711700000000
  }
}
```

### 2.2 登录

```
POST /auth/login
```

请求体：
```json
{
  "email": "user@example.com",
  "password": "Str0ng!Pass"
}
```

### 2.3 刷新 Token

```
POST /auth/refresh
```

请求体：
```json
{
  "refreshToken": "eyJ..."
}
```

### 2.4 登出

```
POST /auth/logout
Authorization: Bearer <token>
```

### 2.5 OAuth2 社交登录

```
GET /auth/oauth/google?redirect=https://app.yourdomain.com/callback
GET /auth/oauth/github?redirect=...
```

---

## 3. 资源 API 模板

> **使用说明**：为每种核心资源复制以下模板，填写 `<!-- FILL -->` 部分。

---

### 3.x <!-- FILL: 资源名称（如 Documents / Files / Posts） -->

基础路径：`/<!-- FILL: resources -->`

#### 列表

```
GET /<!-- FILL: resources -->
Authorization: Bearer <token>
```

查询参数：
| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| page | number | 否 | 页码，默认 1 |
| pageSize | number | 否 | 每页条数，默认 20，最大 100 |
| sort | string | 否 | 排序字段，默认 `created_at` |
| order | string | 否 | asc / desc，默认 desc |
| <!-- FILL: filter --> | string | 否 | <!-- FILL: 过滤条件说明 --> |

响应：`200 OK`
```json
{
  "code": 0,
  "data": [
    {
      "id": "<!-- FILL: res_xxx -->",
      <!-- FILL: 资源字段... -->
      "createdAt": 1711699200000,
      "updatedAt": 1711699200000
    }
  ],
  "pagination": { "page": 1, "pageSize": 20, "total": 50 }
}
```

#### 获取单条

```
GET /<!-- FILL: resources -->/:id
Authorization: Bearer <token>
```

响应：`200 OK`
```json
{
  "code": 0,
  "data": {
    "id": "<!-- FILL: res_xxx -->",
    <!-- FILL: 完整资源字段... -->
  }
}
```

错误：`404` 资源不存在，`403` 无权访问

#### 创建

```
POST /<!-- FILL: resources -->
Authorization: Bearer <token>
Content-Type: application/json
```

请求体：
```json
{
  <!-- FILL: 创建所需字段... -->
}
```

响应：`201 Created`

#### 更新（全量）

```
PUT /<!-- FILL: resources -->/:id
Authorization: Bearer <token>
```

#### 更新（部分字段）

```
PATCH /<!-- FILL: resources -->/:id
Authorization: Bearer <token>
```

请求体：
```json
{
  <!-- FILL: 要更新的字段（只需包含要改的）... -->
}
```

#### 删除

```
DELETE /<!-- FILL: resources -->/:id
Authorization: Bearer <token>
```

响应：`204 No Content`

---

## 4. 核心业务 API（示例：文档模块）

<!-- OPTIONAL: 根据产品实际业务填写，以下为示例 -->

### 4.1 文档 API

| Method | Path | 描述 | 认证 |
|--------|------|------|------|
| GET | /documents | 获取文档列表 | ✅ |
| POST | /documents | 创建文档 | ✅ |
| GET | /documents/:id | 获取文档详情 | ✅ |
| PATCH | /documents/:id | 更新文档 | ✅ |
| DELETE | /documents/:id | 删除文档 | ✅ |
| POST | /documents/:id/duplicate | 复制文档 | ✅ |
| POST | /documents/:id/export | 导出文档（PDF/HTML/MD） | ✅ |
| POST | /documents/:id/share | 创建分享链接 | ✅ |

#### 导出文档

```
POST /documents/:id/export
```

请求体：
```json
{
  "format": "pdf",       // pdf | html | markdown | docx
  "options": {
    "includeTitle": true,
    "theme": "light"
  }
}
```

响应：
```json
{
  "code": 0,
  "data": {
    "downloadUrl": "https://cdn.yourdomain.com/exports/doc_xxx.pdf",
    "expiresAt": 1711786000000
  }
}
```

### 4.2 分享 API

| Method | Path | 描述 | 认证 |
|--------|------|------|------|
| POST | /shares | 创建分享链接 | ✅ |
| GET | /shares/:shareId | 获取分享内容（公开） | ❌ |
| DELETE | /shares/:shareId | 撤销分享 | ✅ |

---

## 5. AI API

<!-- OPTIONAL: 如果产品包含 AI 功能 -->

```
POST /ai/complete
Authorization: Bearer <token>
Content-Type: application/json
```

请求体：
```json
{
  "action": "continue",     // continue | rewrite | summarize | outline | proofread | translate
  "content": "文档内容片段...",
  "context": "可选的上下文...",
  "options": {
    "model": "deepseek-chat",
    "language": "zh-CN",
    "maxTokens": 1000,
    "stream": true
  }
}
```

流式响应（SSE）：
```
data: {"delta": "这是", "done": false}
data: {"delta": "生成的", "done": false}
data: {"delta": "内容", "done": false}
data: {"delta": "", "done": true, "usage": {"promptTokens": 100, "completionTokens": 50}}
```

---

## 6. WebSocket 事件

<!-- OPTIONAL: 如果产品包含实时协作功能 -->

连接地址：`wss://api.<!-- FILL: yourdomain.com -->/v1/ws`

### 认证握手

```json
{
  "type": "auth",
  "token": "eyJ..."
}
```

### 事件列表

| 事件名 | 方向 | 数据格式 | 描述 |
|--------|------|---------|------|
| `doc:join` | C→S | `{docId}` | 加入文档房间 |
| `doc:leave` | C→S | `{docId}` | 离开文档房间 |
| `doc:update` | C→S | `{docId, delta, version}` | 推送编辑变更 |
| `doc:sync` | S→C | `{docId, content, version}` | 同步文档最新内容 |
| `doc:presence` | S→C | `{docId, users: [{id, name, cursor}]}` | 在线用户列表 |
| `notification` | S→C | `{type, message, data}` | 系统通知 |
| `error` | S→C | `{code, message}` | 错误推送 |

---

## 7. 速率限制

| 端点分类 | 限制 | 窗口 |
|---------|------|------|
| 认证接口 | 10 次 | 1分钟 |
| 普通 API | 300 次 | 1分钟 |
| AI 接口 | 20 次 | 1分钟 |
| 导出接口 | 5 次 | 1分钟 |

超出限制响应头：
```
X-RateLimit-Limit: 300
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1711699260
Retry-After: 42
```

---

## 8. SDK 示例

### JavaScript/TypeScript

```typescript
import { ApiClient } from '@<!-- FILL: yourpackage -->/sdk';

const client = new ApiClient({
  baseUrl: 'https://api.yourdomain.com/v1',
  token: localStorage.getItem('access_token'),
});

// 获取文档列表
const { data } = await client.documents.list({ page: 1, pageSize: 20 });

// 流式 AI 续写
for await (const chunk of client.ai.stream('continue', { content: '...', options: { stream: true } })) {
  console.log(chunk.delta);
}
```

### cURL

```bash
# 登录
curl -X POST https://api.yourdomain.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"Str0ng!Pass"}'

# 获取文档列表
curl -X GET "https://api.yourdomain.com/v1/documents?page=1&pageSize=20" \
  -H "Authorization: Bearer eyJ..."
```

---

## 9. Changelog

| 版本 | 日期 | 变更 |
|------|------|------|
| v1.0 | <!-- FILL: YYYY-MM-DD --> | 初始版本 |
