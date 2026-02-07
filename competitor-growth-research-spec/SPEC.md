# Competitor Growth Research — Spec

This spec defines a portable, resumable workflow to produce a competitor growth research report.

It is **agent-agnostic**: it does not assume OpenClaw, Codex, or any particular browser automation tool.

## 1. Output schema (report headings)

Top-level headings (no parentheses):

0) 一句话定位
1) 流量数据
2) 痛点分析
3) 产品拆解
4) 流量拆解
5) 可借鉴行动
6) MRR 粗估
7) 下一步缺口

### Section 1: 流量数据
Required:
- Similarweb: visits + engagement + top country + timeframe.
- Semrush: organic keywords/traffic/cost + **paid keywords/paid traffic (include even if 0)**.

### Section 4: 流量拆解
Use these subheadings:
- 4.1 页面 SEO
- 4.2 站外分发与外链
- 4.3 社媒
- 4.4 广告
- 4.5 其他

Rules:
- Present *what they did* + evidence + short analysis.
- For refdomains: present top refdomains list (top 15+) and note when backlinks are used as a proxy for referral traffic.

## 2. Data sources (allowed)
Typical sources:
- Similarweb
- Semrush
- Public web pages (official site + third-party pages)
- Social (X, Reddit)

If a source is unavailable (login/captcha), mark as a gap.

## 3. Intermediate artifacts (portable)

All runs write artifacts under:

- `runs/<run_id>/`
  - `status.json`
  - `artifacts/<source>/...`
  - `report.md` (optional)

Recommended artifacts:
- `artifacts/similarweb/metrics.json`
- `artifacts/semrush/overview.json`
- `artifacts/semrush/organic_overview_<db>.json`
- `artifacts/semrush/top_pages_<db>.json`
- `artifacts/semrush/top_keywords_<db>.json`
- `artifacts/semrush/refdomains_top.json`
- `artifacts/social/x.json`
- `artifacts/social/reddit.json`

## 4. Run-state + watchdog protocol

See:
- `RUN_STATE.md`
- `WATCHDOG.md`
