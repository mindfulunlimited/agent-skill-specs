# daily-work-summary — SPEC

## Purpose
Write a **truthful, complete daily work summary** for “昨天”(America/Los_Angeles) and **send it to Telegram** so the user can review the previous day.

This exists to prevent “summary drift” when work spans multiple repos/tools (OpenClaw + browser automation + Ops + Obsidian).

## Inputs
- date (optional): defaults to “yesterday” in America/Los_Angeles.

## Required sources (must aggregate)
1) Obsidian daily summary file (if exists):
   - `~/Documents/ObsidianVault/00_System/Daily_Work_Summaries/YYYY-MM-DD.md`
2) OpenClaw durable memory for the day:
   - `~/.openclaw/workspace/memory/YYYY-MM-DD.md`
3) Git activity (same day) for these repos (if present):
   - `~/Documents/ObsidianVault`
   - `~/agent-skill-specs/agent-skill-specs`
   - active product repos touched (e.g. `~/Documents/Projects/*` recent commits)
   - tooling repos touched (e.g. `~/Documents/Tooling/BrowserAutomation/*`)

If a source is missing, the summary MUST say so and fall back to the other sources.

## Output format (Chinese, fixed headings)
【昨日完成】
- ...

【阻塞/风险】
- ...

【今日重点】
- ...

## Rules
- Must include major milestones across: browser automation stability, skill changes, product repo changes, ops/governance decisions.
- Must not invent accomplishments.
- Must not include passwords/secrets.

## Telegram delivery (non-negotiable)
- Always send to Telegram target `6475983054`.
- Message must end with: `已同步到 GitHub`.
- If git push failed, say so explicitly and DO NOT claim synced.

## Evidence pointers
When relevant, include key file paths + commit hashes.
