# RUN_STATE.md (toolify-scout-spec v0.1)

## Purpose
Persist minimal state across runs to:
- enforce **not-too-new** filtering
- avoid reprocessing the same domains too frequently
- support weekly reporting (deltas, new entrants)

## State files (recommended)
Store as JSON in the adapter’s workspace, e.g.:
- `run-state/state.json`
- `run-state/seen-domains.ndjson`

## Minimum state schema
```json
{
  "schema_version": "0.1",
  "created_utc": "…",
  "updated_utc": "…",
  "first_seen": {
    "example.com": "2025-11-10T12:34:56Z"
  },
  "last_checked": {
    "example.com": "2026-02-07T12:00:00Z"
  },
  "last_published_week": "2026-W06",
  "published": {
    "2026-W06": ["example.com", "another.com"]
  },
  "config_overrides": {
    "min_age_days": 45
  }
}
```

### Notes
- `first_seen` is set when a domain is first extracted from Toolify, even if it fails validation.
- `last_checked` is updated when Similarweb/Semrush validations are attempted.
- `published` stores prior weekly selections for change tracking.

## Retention
- Keep `first_seen` indefinitely (or at least 2 years).
- Keep detailed per-run raw HTML snapshots only for 2–4 weeks.

## Determinism
A weekly run should be reproducible given:
- the stored candidate set for that week (raw snapshots)
- the config parameters

(Perfect determinism is not expected due to third-party metric drift.)
