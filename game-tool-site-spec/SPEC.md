# 游戏工具站（Game Tool Site, Next.js）- SPEC

> 类别：新词快站（子类：游戏工具站）
> 
> 核心理念：不是套模板内容站，而是做“用户离不开”的工具型站点；首页必须极致 SEO。

## 目标
给定一个新兴趋势词（target/keyword）和域名（产品 id），快速产出一个可部署的 Next.js 工具站：
- 解决明确用户痛点（不止计算器/占位页）
- 首页为强 SEO Landing（结构化数据/内部链接/速度/可索引性到位）
- 支持多页面信息架构（按需扩展）

## 输入（用户提供）
**必填**
- domain: string（产品 id，例如 `burglingnomes.online`）
- target: string（目标词）
- keyword: string（主关键词；可与 target 相同或补充）
- **anchorLink: string（权威锚点链接）**
  - 必须是“能唯一指向该游戏/产品”的权威页面：例如 Steam/Epic/App Store/Google Play/官方站/官方 Wiki/Roblox 游戏页。
  - 目的：防止名称撞车导致 research / media library 跑偏。

**默认值（除非用户覆盖）**
- language: English
- geo: Worldwide

## 可选输入（由 agent 自动补全；用户可覆盖）
- searchIntent: 用户搜索意图（jobs-to-be-done）
- officialLinks: 游戏官网/权威链接
- competitors: 竞品/参考站 1-5 个
- featureScope: 核心功能范围（MVP+后续）

## 输出（交付物）
1) 本地项目目录（按产品编号）
- Location: `~/Projects/<P####>_<domain>/`
- Tech: Next.js（App Router 优先；TypeScript）

2) GitHub repo（已存在则复用）
- Owner: 默认 `mindfulunlimited`（可覆盖）
- Repo: 由产品建档 skill 创建（例如 `burglingnomes-online`）
- 项目应 push 到该 repo（main 分支或 feature 分支，按约定）

3) SEO 强首页 + 多页面信息架构
- 首页：
  - title/description/h1
  - FAQ / How-to / SoftwareApplication 等 schema（按适用）
  - internal links（指向核心工具页与信息页）
  - 性能（Lighthouse/核心 Web 指标为目标）
  - robots / sitemap / canonical
- 站内页面（最小集合）：
  - `/tool` 或核心功能入口页（可为首页集成）
  - `/about` `/privacy` `/terms`
  - `/faq`（可合并到首页）

4) IA（信息架构）与验收清单
- `docs/IA.md`
- `docs/SEO_CHECKLIST.md`

## 需求/调研责任（推荐默认）
- **主会话（OpenClaw 主智能）负责“证据链/数据源”**：
  - 从 `anchorLink` 抽取权威事实（玩法/平台/模式/更新时间/官方称呼）。
  - 建立 `docs/MEDIA_LIBRARY.md`（YouTube/社区素材）并用可验证工具（如 `yt-dlp` 元数据）固化，避免幻觉与跑偏。
  - 产出关键词意图分组 + IA 草案（codes/tier list/builds/wiki/calculator 等）。
- **coding agent 负责“实现”**：基于上述证据链与 IA，将页面/工具/内链落地到 Next.js 工程。
- 允许 coding agent 自行补充公开调研，但必须通过 `anchorLink` 做一致性校验。

## Subagent 使用建议（重要）
- **不建议将“research + media library + 产品定位”交给全自动 subagent**（高歧义/易撞车/难验真）。
- 可用 subagent 的部分：在锚点锁死后做“扩充/整理/表格化初稿”，但最终以主会话验收+落盘为准。

## 断点（建议 run 状态机；MVP 可先人工）
- ensure_repo
- research
- scaffold_nextjs
- implement_core_tool
- implement_seo_landing
- add_sitemap_robots
- build_verify
- push_repo
- (deploy - 可选，后续由部署 skill 接管)

## 验收标准（MVP）
- `npm run build` 或 `pnpm build` 成功
- 首页与核心工具可用（非占位）
- sitemap/robots/canonical 正常
- 页面 metadata + schema 存在
- push 到 GitHub private repo 成功

## 备注
- 后续扩展：程序化生成二/三级内页（另一个 skill 负责，不与本 spec 强绑定）。
