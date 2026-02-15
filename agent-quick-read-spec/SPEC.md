# agent-quick-read — SPEC

## Purpose
Provide a systematic, repeatable rapid-alignment protocol for any new/parallel agent so execution starts from full governance context (not ad-hoc patches).

## Inputs
- Session context (main/group/subagent)
- Optional target project/domain

## Required sources by layer

### Layer 0 (always)
- `AGENTS.md`, `SOUL.md`, `USER.md`
- `memory/<today>.md`, `memory/<yesterday>.md`
- `MEMORY.md` (main session only)

### Layer 1 (always)
- Global baseline: `~/.openclaw/workspace/EXECUTION_BASELINE.md`
- Script guard registry: `~/.openclaw/workspace/SCRIPT_REGISTRY.md`
- Browser policy: `~/Documents/Operations/Tooling/BrowserAutomation/CDP_POLICY.md`
- Account mapping policy: `~/Documents/Operations/Accounts/SEO_Tools_3ue.md`
- Shared workflow governance: `~/.openclaw/skills/linear-issue-ops/SKILL.md`

### Layer 2 (always scan)
- Obsidian skill index: `~/Documents/ObsidianVault/00_System/OpenClaw/Skills/`
- Managed skill + spec for current task:
  - `~/.openclaw/skills/<skill>/SKILL.md`
  - `~/agent-skill-specs/agent-skill-specs/<skill>-spec/SPEC.md`

### Layer 3 (task-specific)
- Active Linear issues (especially assigned to liangcl2026@gmail.com)
- Product dossier (`docs/PRODUCT_PROFILE.json`) for target work

## Output
Execution Brief (concise):
1. Current objective
2. Non-negotiable constraints
3. System task slots (stable taxonomy, dynamic fill):
   - Platform reliability
   - Data quality & attribution
   - Discoverability & indexing
   - Asset/account governance
   - Delivery & communication
4. Active tasks mapping (issue IDs + next step)
5. Risks/blockers
6. Immediate next action
7. Policy checks passed

## Acceptance
- Brief exists before any implementation.
- `EXECUTION_BASELINE.md` is explicitly loaded and acknowledged.
- Policy checks explicitly include browser/account/linear/delivery/concurrency.
- No fabricated context.
- If policy source missing, run is blocked and missing source is named.
- Uses stable system task slots; does not hardcode stale one-off tasks as persistent policy.
