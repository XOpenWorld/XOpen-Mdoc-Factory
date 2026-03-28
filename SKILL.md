---
name: xopen-mdoc-factory
description: "XOpen Mdoc Factory — Generate a complete project documentation package from a product idea. Triggered by /mdoc command or natural language like '生成文档', '文档工厂', 'doc factory'. Supports 18 document types across 7 layers: PRD, tech stack, architecture, API spec, database schema, security design, design system, UI specification, DevOps guide, and more. Use /mdoc [product] for full doc set, or /mdoc [product] [type] for a single document."
---

# XOpen Mdoc Factory — 项目文档工厂

Generate a complete, structured project documentation package from a single product idea or requirement description.

## Purpose

Transform a product idea into a professional-grade documentation package containing 18 types of documents across 7 layers. Each document follows industry-standard structure, uses consistent formatting, and is ready for version control.

## Command Syntax (Slash Commands)

Users can invoke this skill with the `/mdoc` command:

```
/mdoc [product]               # Generate the full documentation set for a product
/mdoc [product] [type...]     # Generate one or more specific documents
```

**Product name** is flexible — use any short identifier:
```
/mdoc flow                    # XOpen Flow 完整文档集
/mdoc mdoc prd                # XOpen Mdoc 的 PRD
/mdoc agent tech api db       # XOpen Agent 的技术选型 + API + 数据库（多文档）
/mdoc "my-saas-app"           # 任意产品名
```

**Supported `[type]` values:**

| Alias | Document Generated |
|-------|--------------------|
| `prd` | Product Requirements Document |
| `vision` | Product Vision |
| `roadmap` | Development Roadmap |
| `competitive` | Competitive Analysis |
| `onboarding` | Onboarding Design |
| `design` | Design System |
| `ui` | UI Specification |
| `component` | Component Library Spec |
| `tech` | Tech Stack Specification |
| `arch` | Architecture Design |
| `api` | API Specification |
| `db` | Database Schema |
| `security` | Security Design |
| `devops` | DevOps Guide |
| `test` | Testing Strategy |
| `i18n` | i18n Specification |
| `contributing` | Contributing Guide |
| `changelog` | Changelog |

**Rules:**
- `/mdoc [product]` with no type → generate **full documentation set** (all Required docs)
- `/mdoc [product] [type]` → generate **single document**, skip requirements gathering
- `/mdoc [product] [type1] [type2] ...` → generate **multiple documents** in one batch
- If product name matches a known XOpen product (mdoc, flow, agent, proxy, cloud), auto-load context from memory

## When to Use

Trigger this skill when:

- User types `/mdoc` command (highest priority — parse command and execute immediately)
- User mentions "doc factory", "document factory", "文档工厂", "生成文档", "文档包", "mdoc factory"
- User says "帮我写/生成/补全 [产品名] 的 [文档类型]"（e.g., "帮我写 Flow 的 API 文档"）
- User says "给 [产品名] 生成 [文档类型]"（e.g., "给 XOpen Agent 生成 PRD"）
- User says "基于 [产品名] 写 [文档类型]"（e.g., "基于 Mdoc 生成技术选型文档"）
- User asks "help me write project docs" or "帮我写项目文档"

## Document Types (18 documents, 7 layers)

### Layer 1: Strategy (战略层)

| # | Document | Description | Priority |
|---|----------|-------------|----------|
| 1 | **Product Vision** (产品图景) | Brand positioning, product matrix, user personas, business model, competitive analysis | Required |
| 2 | **Roadmap** (开发路线图) | Timeline, milestones, priority matrix, phased delivery plan | Required |

### Layer 2: Product (产品层)

| # | Document | Description | Priority |
|---|----------|-------------|----------|
| 3 | **PRD** (产品需求文档) | Feature list, user stories, acceptance criteria, scope definition | Required |
| 4 | **Competitive Analysis** (竞品分析) | Competitor matrix, feature comparison, differentiation strategy | Optional |
| 5 | **Onboarding Design** (用户引导设计) | First-run experience, empty states, tooltips, help system | Optional |

### Layer 3: Design (设计层)

| # | Document | Description | Priority |
|---|----------|-------------|----------|
| 6 | **Design System** (设计规范) | Color tokens, typography, spacing, shadows, borders, breakpoints | Required |
| 7 | **UI Specification** (UI 交互规范) | Page-by-page interaction flows, states, animations, responsive behavior | Required |
| 8 | **Component Library Spec** (组件库规范) | Component API (Props/Events/Slots), variants, accessibility | Optional |

### Layer 4: Engineering (技术层)

| # | Document | Description | Priority |
|---|----------|-------------|----------|
| 9 | **Tech Stack** (技术选型) | Framework/tool comparison, decision rationale, alternatives | Required |
| 10 | **Architecture Design** (技术架构) | Module diagram, data flow, service boundaries, integration points | Required |
| 11 | **API Specification** (API 接口文档) | REST/WebSocket endpoints, request/response schemas, error codes | Required |
| 12 | **Database Schema** (数据库设计) | ER diagram, DDL, indexes, RLS policies, migration strategy | Required |
| 13 | **Security Design** (安全设计) | Auth, encryption, CSRF/XSS/SQLi防护, compliance, audit | Required |

### Layer 5: Operations (运维层)

| # | Document | Description | Priority |
|---|----------|-------------|----------|
| 14 | **DevOps Guide** (DevOps 指南) | CI/CD pipeline, environments, deployment, monitoring, cost estimation | Optional |
| 15 | **Testing Strategy** (测试策略) | Test pyramid, coverage targets, E2E scenarios, mock strategy | Optional |

### Layer 6: Internationalization (国际化层)

| # | Document | Description | Priority |
|---|----------|-------------|----------|
| 16 | **i18n Specification** (国际化规范) | Language priorities, locale formats, translation workflow, glossary | Optional |

### Layer 7: Community (社区层)

| # | Document | Description | Priority |
|---|----------|-------------|----------|
| 17 | **Contributing Guide** (贡献指南) | Development setup, coding standards, PR process, commit conventions | Optional |
| 18 | **Changelog** (更新日志) | Version history, release notes format (Keep a Changelog) | Optional |

## Workflow

### Step 1: Gather Requirements

Ask the user for (or extract from context):

1. **Product name** — What is the product called?
2. **Product type** — Web app / Mobile app / Desktop app / API / Library / SaaS / Other
3. **Core idea** — One-paragraph description of what it does and for whom
4. **Target users** — Who will use it? (developers, enterprises, consumers, etc.)
5. **Key features** — List of 3-10 core features (can be brief)
6. **Tech preferences** (optional) — Any preferred languages, frameworks, or tools
7. **Business model** (optional) — How will it make money? (subscription, freemium, ads, etc.)
8. **Document scope** — Which documents to generate? Default: all Required + user-chosen Optional

Present the extracted information back for user confirmation before proceeding.

### Step 2: Generate Documents

After user confirms the requirements outline:

1. **Generate in parallel batches** — Documents within the same layer can be generated simultaneously
2. **Use the template references** — Each document type has a template in `references/`. Read the relevant template before writing.
3. **Cross-reference between documents** — Ensure consistency (e.g., tech stack mentioned in PRD matches the Tech Stack doc)
4. **Write in the user's language** — Match the language the user used in their request (Chinese or English)

**Batch order**:
```
Batch 1 (Strategy + Product):  Product Vision, Roadmap, PRD, Competitive Analysis
Batch 2 (Design):              Design System, UI Specification, Component Library
Batch 3 (Engineering):         Tech Stack, Architecture, API Spec, Database Schema, Security
Batch 4 (Operations+):         DevOps, Testing, i18n, Contributing, Changelog
```

### Step 3: Review & Refine

After all documents are generated:

1. **Cross-check consistency** — Verify tech stack, features, and terminology are consistent across all docs
2. **Ask user for feedback** — "文档已生成完毕，是否需要调整某个部分？"
3. **Apply refinements** — Modify individual documents based on user feedback

### Step 4: Package Output

Generate the final folder structure:

```
{product-name}-docs/
├── 00-README.md                         ← Document index + quick start
├── 01-strategy/
│   ├── product-vision.md
│   └── roadmap.md
├── 02-product/
│   ├── prd.md
│   ├── competitive-analysis.md
│   └── onboarding-design.md
├── 03-design/
│   ├── design-system.md
│   ├── ui-specification.md
│   ├── component-library.md
│   └── design-tokens.json               ← Optional: JSON token file
├── 04-engineering/
│   ├── tech-stack.md
│   ├── architecture.md
│   ├── api-specification.md
│   ├── database-schema.md
│   └── security-design.md
├── 05-operations/
│   ├── devops-guide.md
│   └── testing-strategy.md
├── 06-i18n/
│   └── i18n-specification.md
└── 07-community/
    ├── contributing-guide.md
    ├── changelog.md
    └── code-of-conduct.md
```

Create the `00-README.md` index file that:
- Lists all documents with brief descriptions
- Provides the project overview summary
- Includes a "how to use these documents" section

**Output location**: Ask the user for preferred output path. Default: `{workspace}/docs/` or `{workspace}/{product-name}-docs/`

## Template References

Before generating each document, read the corresponding template from `references/`:

| Document | Template File |
|----------|--------------|
| Product Vision | `references/product-vision.md` |
| Roadmap | `references/roadmap.md` |
| PRD | `references/prd.md` |
| Competitive Analysis | `references/competitive-analysis.md` |
| Design System | `references/design-system.md` |
| UI Specification | `references/ui-specification.md` |
| Tech Stack | `references/tech-stack.md` |
| Architecture | `references/architecture.md` |
| API Specification | `references/api-specification.md` |
| Database Schema | `references/database-schema.md` |
| Security Design | `references/security-design.md` |
| DevOps Guide | `references/devops-guide.md` |
| Testing Strategy | `references/testing-strategy.md` |
| i18n Specification | `references/i18n-specification.md` |
| Contributing Guide | `references/contributing-guide.md` |
| Changelog | `references/changelog.md` |

Each template contains:
- **Required sections** — Must-have sections with field descriptions
- **Optional sections** — Nice-to-have sections
- **Formatting guidelines** — Markdown structure and style
- **Quality checklist** — What makes this document excellent

## Quality Standards

### General Rules

- Use consistent Markdown formatting across all documents
- Include a version number and date in each document header
- Use tables for structured data (comparisons, matrices, status tracking)
- Use Mermaid diagrams for flows, architectures, and timelines
- Use code blocks with language tags for technical content
- Keep each document focused — avoid overlap between documents
- Use `> ` blockquotes for important notes and warnings

### Consistency Checklist

- [ ] Product name is identical across all documents
- [ ] Feature names match between PRD, Roadmap, and Architecture
- [ ] Tech stack in PRD matches Tech Stack document
- [ ] API endpoints align with Database Schema entities
- [ ] User personas are consistent between PRD and Onboarding Design
- [ ] Design tokens in Design System match UI Specification references

## Language Support

- **Chinese (default)**: Generate documents in Chinese when user communicates in Chinese
- **English**: Generate documents in English when user communicates in English
- **Bilingual**: When in doubt, ask user preference
- Technical terms (API, SDK, CI/CD, etc.) remain in English regardless of language

## Customization

### For Software Products

Automatically include all 7 layers. Prioritize Engineering layer (5 Required docs).

### For Non-Software Projects

Adjust the Engineering layer — keep Tech Stack and Architecture but simplify/remove API Spec and Database Schema unless applicable. Add domain-specific documents as needed.

### For MVP/Lean Startups

Default to Required documents only (~10 docs). Skip Optional documents unless user requests them.

### For Enterprise Projects

Include all 18 documents. Add Compliance subsection to Security Design. Add SLA to DevOps Guide.
