# Skill Registry（skills 管理/归档同步）- SPEC

## 目标
避免用户反复提醒：当新增/修改 skills 时，自动把三个地方对齐：
1) OpenClaw 实现目录（workspace/skills）
2) Spec 仓库（agent-skill-specs）
3) Obsidian 的 Roadmap/索引/单页档案

## 输入
- 无（默认扫描固定目录）
- 可选参数（后续扩展）：
  - obsidianVaultPath
  - roadmapPath
  - skillDocsDir

## 扫描范围（默认）
- Impl skills：`/Users/qianliu/.openclaw/workspace/skills/*/SKILL.md`
- Specs：`/Users/qianliu/agent-skill-specs/agent-skill-specs/*-spec/`
- Cron jobs：`/Users/qianliu/.openclaw/cron/jobs.json`（用于 watchdog 标识）

## 输出
- 更新 Obsidian：`/Users/qianliu/Documents/ObsidianVault/00_System/OpenClaw/Roadmap_Skills.md`
  - 生成/更新一张总表：skill 名、impl 路径、spec 路径、watchdog 是否存在。
- （可选）为缺失的 skill 单页生成 stub：
  - `00_System/OpenClaw/Skills/<skill-name>.md`

## Watchdog 判定规则（MVP）
- 若 cron jobs.json 中存在 name 以 `watchdog:` 开头 且 message 或 name 中包含该 skill 名 → 记为 ✅
- 否则记为 —

## 幂等
- 重复运行不产生重复条目；以“扫描结果”为准覆盖表格区块。

## 验收标准
- 新增一个 skill 目录后运行一次：Roadmap 自动出现一行；缺失单页自动生成。
- 修改/新增 watchdog cron 后运行一次：Watchdog 列自动更新。
