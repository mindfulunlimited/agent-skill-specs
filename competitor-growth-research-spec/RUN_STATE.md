# Run State Protocol

Purpose: enable resumable, monitorable multi-step runs across agents.

## status.json (required)

Path: `runs/<run_id>/status.json`

### Fields
- `runId` string
- `spec` string (e.g. `competitor-growth-research-spec`)
- `target` object
  - `domain` string
  - `market` string (optional, e.g. `JP`)
- `state` enum: `queued | running | needs_human | blocked | done | failed`
- `step` string (current step id)
- `steps` array of step objects
  - `id` string
  - `state` enum: `pending | running | done | failed | skipped`
  - `startedAt` ISO
  - `endedAt` ISO
  - `attempts` number
  - `lastError` string (optional)
  - `artifacts` array of relative paths produced
- `startedAt` ISO
- `updatedAt` ISO
- `lastError` string (optional)
- `needsHuman` boolean
- `humanMessage` string (optional)
- `resumeHint` string (optional)

## Step ids (recommended)
- `similarweb.collect`
- `semrush.overview.collect`
- `semrush.organic_overview.collect`
- `semrush.top_pages.collect`
- `semrush.top_keywords.collect`
- `semrush.refdomains.collect`
- `social.x.collect`
- `social.reddit.collect`
- `report.render`

## Semantics
- Any agent can resume a run by reading `status.json`, locating the first `pending` step, executing it, writing artifacts, and updating the step state.
- If login/captcha is required, set `state=needs_human`, `needsHuman=true`, and fill `humanMessage` with exact instructions.
