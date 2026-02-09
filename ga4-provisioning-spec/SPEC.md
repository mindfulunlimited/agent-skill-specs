# GA4 Provisioning Spec (v0)

## Goal
Given a website domain (e.g. `mrrfast.com`), provision a **new GA4 Property** + **Web data stream** in Google Analytics, then return:

- `measurementId` (format `G-XXXXXXXXXX`)
- `gtag.js` snippet (install code)

And **send the measurementId + snippet to Telegram** (target user chat: `6475983054`).

## Inputs
- `domain` (required): apex domain or host. Examples: `mrrfast.com`, `app.mrrfast.com`.

## Derived defaults
- `propertyName`: default to domain apex (e.g. `mrrfast`) or user-provided override.
- `streamName`: default `${propertyName}` or `${domain}`.
- `urlProtocol`: default `https://`.
- `tz`: `America/Los_Angeles`.
- `currency`: `USD`.

## Output contract
Return a JSON-like summary:

```json
{
  "domain": "mrrfast.com",
  "propertyName": "mrrfast",
  "streamUrl": "https://mrrfast.com",
  "measurementId": "G-...",
  "gtagSnippet": "<script ...>...",
  "telegramTarget": "6475983054"
}
```

## Resumability
The workflow must be resumable. Persist state on disk:

- `runs/ga4_<domain>_<YYYY-MM-DD>/status.json`
- `runs/ga4_<domain>_<YYYY-MM-DD>/artifacts/measurement.json`

`status.json` includes `state` and step statuses (`pending|running|done|needs_human|error`).

## Human-in-the-loop gates
Set `needs_human` if any of:
- Google login not completed
- captcha / suspicious login
- permission denied / cannot create property
- UI changed such that elements cannot be located

## Telegram delivery
Send exactly the measurement id and gtag snippet; avoid extra commentary.

## Non-goals
- No GTM container creation
- No Obsidian writes
- No DNS changes
