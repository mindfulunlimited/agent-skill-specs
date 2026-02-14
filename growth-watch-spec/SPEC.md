# growth-watch — SPEC

## Purpose
Daily/weekly **growth monitoring** that explains *what changed and why* using **GSC + GA4** (and optionally other sources later).

This is NOT troubleshooting (health fixes). It is a **performance digest**: winners/losers, drivers, and next actions.

## Inputs
- product/domain (can be multiple)
- mode:
  - `daily_yesterday_vs_7day_avg` (primary)
  - `weekly_7d_vs_prev7d` (secondary)
- required comparison windows (always include both in outputs):
  - `last_7d_vs_prev_7d`
  - `last_28d_vs_prev_28d`
- time range defaults:
  - daily: yesterday vs prior 7-day average
  - plus mandatory context from 7d and 28d comparisons
  - weekly: last 7 days vs previous 7 days + 28d context

## Account mapping (hard gate)
- New standard (future projects): GA + GSC use `liangcl2026@gmail.com`.
- Legacy default (unless dossier overrides):
  - GA: `yddcreation@gmail.com`
  - GSC: `qianliuster@gmail.com`
Hard rule: before any actions, verify top-right Google account email matches the required one.

## Required sources
- GSC Performance:
  - Queries, Pages, Countries, Devices, Search appearance (when available)
- GA4 Reports:
  - Realtime snapshot
  - Acquisition (traffic acquisition)
  - Pages & screens
  - Outbound clicks if instrumented; otherwise output as a tracking gap

## Output (Chinese, must be concrete)
For each product:

### 0) Summary (numbers)
- Clicks/Impressions/CTR/Position (delta vs baseline)
- Users/Sessions/Engaged sessions/Avg engagement time (delta vs baseline)

### 1) GSC Drivers
- Top 10 Query changes (delta table, with landing page when possible)
- Top 10 Page changes (delta table, with top queries)
- Country/Device anomalies
- Search appearance notes (rich results gained/lost)

### 2) GA Drivers
- Channel changes (Organic/Direct/Referral/Social)
- "Is this us?" diagnosis with evidence (realtime location/device/source; suspicious referrals; engagement patterns)

### 3) Actions (must be specific)
- 3 concrete actions with:
  - target query/page
  - change proposal (title/module/internal links/new page)
  - acceptance signals to watch in GSC/GA

## Safety
- No secrets in outputs.
- No automatic fixes; if severe anomalies found, recommend running `gsc-troubleshoot` oneshot.

## Browser/runtime (hard rule)
- Must run on **Real Google Chrome Stable via CDP attach**.
- Do NOT spawn Chrome for Testing / chrome-headless-shell.
- Preferred CDP port for growth/ops: **9223**.
- If CDP attach fails, stop and request HITL (login/2FA). Do not fall back.

## Delivery
- Daily digest can be sent to Telegram.
- Weekly digest can be written to Obsidian and optionally sent.
