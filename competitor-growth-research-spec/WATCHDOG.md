# Watchdog Protocol

Goal: a single watchdog can monitor **all** specs/runs by reading `runs/**/status.json`.

## Responsibilities

1) Detect stalled runs
- If `state=running` and `updatedAt` is older than a threshold (e.g. 5–10 min), consider stalled.

2) Auto-resume policy
- If stalled and not `needsHuman`, the watchdog may trigger a resume attempt (agent-specific) up to N times.
- Record resume attempts in `status.json` (increment step attempts, update `lastError`).

3) Human intervention
- If `state=needs_human`, send `humanMessage` to the operator.
- After the human resolves it, the operator or watchdog sets `state=running` and continues.

4) Completion
- When `state=done`, optionally send a completion message with a link to `report.md` and artifact folder.

## Agent-specific notes

The watchdog itself is agent/tool dependent (OpenClaw cron job, systemd timer, etc.), but the **contract** is this file + `status.json`.
