# 产品建档（Product Dossier）- SPEC

## 目标
当用户在 Namecheap 或其他注册商完成域名购买后，用户只需把信息发给助手：
- 产品编号（递增；当前用户已到 63）
- 产品分类：品牌站｜快词站｜自用
- 域名（即产品 id）
- 可选：一句话定位/备注、购买平台、到期日等

助手完成两件事：
1) 在 Obsidian 为该产品建立建档页（并更新索引）。
2) 在 GitHub（默认用户 mindfulunlimited，可覆盖）创建一个以“产品 id=域名”为名的 private repo（用于后续站点/文档/脚本沉淀）。

## 输入
必填：
- productNumber: number（例如 63）
- category: oneOf(品牌站, 快词站, 自用)
- productId: string（域名，例如 example.com）

选填：
- tagline: string（一句话定位）
- registrar: string（namecheap 等）
- purchasedAt: date
- renewAt: date
- notes: string
- githubOwner: string（默认 mindfulunlimited）

## 输出（交付物）
- Obsidian 建档页：一页 Markdown
- Obsidian 产品索引：追加/更新一条记录
- GitHub private repo：创建成功（返回 repo URL）

## Obsidian 落盘约定（默认，可调整）
- 产品档案目录：`10_Verticals/SaaS_Projects/Products/`
- 产品页面文件名：`P<productNumber:zeroPad4>_<productId>.md`（例如 `P0063_example.com.md`）
- 产品索引：`10_Verticals/SaaS_Projects/Products/Products_Index.md`

## 产品页面模板（建议）
- 标题：`P<编号> <域名> — <分类>`
- 基本信息：编号/分类/域名/注册商/购买日/到期日
- 定位与假设：tagline/目标用户/核心关键词
- 技术与仓库：GitHub repo 链接、部署平台（待填）
- 里程碑/待办：后续 skills 的执行记录

## GitHub 行为
- 使用 gh CLI 创建 repo（private）
- repoName：默认将 productId（域名）中的 `.` 替换为 `-`（例如 `example-com`）；并在 Obsidian 记录 repoName 与原始域名的映射。
- 默认 owner = mindfulunlimited（可被输入覆盖）

## 断点/幂等
- 若 Obsidian 已存在该产品页：不重复创建，只补齐缺失字段并更新索引。
- 若 GitHub repo 已存在：只记录链接并跳过创建。

## 安全/人工确认
- 创建 GitHub repo 属于外部动作：默认执行，但在首次运行时应提示用户确认 owner/repoName/private。

## 验收标准
- Obsidian：新建档案页 + 索引新增 1 行
- GitHub：private repo 可访问且名称/owner 正确
