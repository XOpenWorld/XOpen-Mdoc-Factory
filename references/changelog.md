# 📋 更新日志 (Changelog)

> **📌 模板说明**: 本文档记录每个版本的功能变更、Bug 修复和安全更新。遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) 格式。

---

## 🗂️ 元数据

| 字段 | 内容 |
|------|------|
| 📦 产品 | <!-- FILL: Product Name --> |
| 📏 格式 | [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/) |
| 🔖 版本规范 | [Semantic Versioning 2.0.0](https://semver.org/lang/zh-CN/) |

---

## 🔢 版本号说明

```
MAJOR.MINOR.PATCH

MAJOR：不兼容的 API 变更（重大破坏性更新）
MINOR：向后兼容的新功能
PATCH：向后兼容的 Bug 修复

预发布版本: 1.0.0-alpha.1 / 1.0.0-beta.2 / 1.0.0-rc.1
```

---

## 🏷️ 变更类型说明

| 类型 | 📝 说明 |
|------|--------|
| `Added` | ✨ 新功能 |
| `Changed` | 🔄 已有功能的变更 |
| `Deprecated` | ⚠️ 即将废弃的功能 |
| `Removed` | ❌ 已移除的功能 |
| `Fixed` | 🐛 Bug 修复 |
| `Security` | 🔒 安全漏洞修复 |

---

## 🚧 [Unreleased]

> 已合并但尚未发布的变更

### ✨ Added
- <!-- FILL: 新增功能描述 -->

### 🐛 Fixed
- <!-- FILL: 修复问题描述 -->

---

## 🔖 [<!-- FILL: 版本号，如 1.0.0 -->] - <!-- FILL: YYYY-MM-DD -->

### ✨ Added
- 🎉 <!-- FILL: 首个主要功能，例：Markdown 实时预览 -->
- ✨ <!-- FILL: 次要功能 -->

### 🔄 Changed
- <!-- FILL: 变更描述（附 PR 链接或 Issue 编号） -->

### 🐛 Fixed
- <!-- FILL: 修复描述 --> ([#123](https://github.com/<!-- FILL: yourorg/yourrepo -->/issues/123))

### 🔒 Security
- <!-- FILL: 安全修复描述 -->

---

<!-- 以下为示例版本历史，实际使用时替换为真实版本 -->

## [0.1.0] - <!-- FILL: YYYY-MM-DD -->

### ✨ Added
- 基础 Markdown 编辑器（CodeMirror 6）
- 实时预览（remark + rehype）
- 文档 CRUD（创建/读取/更新/删除）
- 文件夹管理
- 本地存储持久化（Zustand + localStorage）
- 深色主题
- 分栏/纯编辑/纯预览模式切换

---

## ⚙️ 自动化 Changelog 生成

使用 `standard-version` 或 `release-please` 自动生成：

```bash
# 安装
npm install -D standard-version

# 生成 changelog + 打 Tag
npm run release        # → patch 版本（0.1.0 → 0.1.1）
npm run release:minor  # → minor 版本（0.1.0 → 0.2.0）
npm run release:major  # → major 版本（0.1.0 → 1.0.0）
```

`package.json` 配置：
```json
{
  "scripts": {
    "release": "standard-version",
    "release:minor": "standard-version --release-as minor",
    "release:major": "standard-version --release-as major"
  },
  "standard-version": {
    "types": [
      {"type": "feat", "section": "Added"},
      {"type": "fix", "section": "Fixed"},
      {"type": "chore", "hidden": true},
      {"type": "docs", "hidden": true},
      {"type": "style", "hidden": true},
      {"type": "refactor", "section": "Changed"},
      {"type": "perf", "section": "Changed"},
      {"type": "test", "hidden": true}
    ]
  }
}
```

---

## 🔗 版本历史链接

[Unreleased]: https://github.com/<!-- FILL: yourorg/yourrepo -->/compare/v<!-- FILL: 最新版本 -->...HEAD
[<!-- FILL: 版本号 -->]: https://github.com/<!-- FILL: yourorg/yourrepo -->/releases/tag/v<!-- FILL: 版本号 -->

---

> **✅ 质量检查**: ✓ 遵循 Keep a Changelog 格式 ✓ 版本号符合 SemVer ✓ 变更类型分类清晰 ✓ 关联 Issue/PR
