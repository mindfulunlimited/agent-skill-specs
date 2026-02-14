# gsc-optimizer — SPEC

## 目标
根据 Google Search Console (GSC) 的问题做**产品层面**修复：sitemap/robots/canonical/indexing/structured data/performance。

并把“账号/项目路径/证据”写入产品档案（product-dossier），避免反复询问。

## 输入
- 产品标识：域名 或 shortName（如 `bnptv`）或 P####
- 可选：明确 GSC 登录邮箱（若用户提供则直接使用）

## 输出
- 1) 直接在 repo 内完成安全的产品层修复（可 commit）
- 2) `docs/CODING_AGENT_GSC_START.md`（paste-ready 给 coding agent 的提示词，包含 Gate + checklist）
- 3) 更新 `docs/PRODUCT_PROFILE.json`（若已存在）或提示先跑 product-dossier

## 执行流程（硬规则）
### Gate 0：定位项目 + canonical base
- 必须定位项目目录（`~/Documents/Projects/...`）
- 必须读：
  - `.env*` / `cloudflare-pages.md`（如有）
  - `lib/constants.*`（SITE_URL）
  - `app/layout.*`（metadataBase/canonical）
  - `app/robots.*` / `app/sitemap.*`

### Gate 1：本地先排雷（不打开 GSC 也能发现的致命问题）
- `public/robots.txt` 覆盖 app 路由
- `public/sitemap.xml` 覆盖 app 路由
- sitemap/canonical 指向旧域名
- www 子域 5xx（多为 Cloudflare Pages custom domains / SSL）

### Gate 2：进入 GSC 拉问题清单（agent-browser）
- Pages（Indexing）：Top reasons + example URLs
- Sitemaps：提交/抓取状态
- Enhancements：FAQ/Breadcrumb 等
- Core Web Vitals：若有数据

### Gate 3：修复策略
- 产品层：代码/配置/重定向/结构化数据 → 由本 skill 直接修
- 非产品层：内容薄/重复 → 输出给 coding agent 的任务

### Gate 4：验证
- `npm run build` 必须 0 errors
- sitemap/robots 在线可访问且域名一致
- 在 GSC 点：Validate fix（针对 404/5xx 等）

## 账号策略（best practice）
- 不做“迁移=删除 property”。
- 采用“共享所有权”：在 GSC Settings → Users and permissions 添加 Owner。

## 适配 Cloudflare Pages（常见）
- 若 Next.js `output: 'export'`：middleware 不生效；用 `public/_redirects`（Pages）做 301。
