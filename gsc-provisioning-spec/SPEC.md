# GSC Provisioning (resumable) — SPEC

## Summary
Given a domain (e.g. `animeparadox.online`), create a Google Search Console property, verify ownership (DNS TXT preferred), submit `sitemap.xml`, then send the result to Telegram.

## Inputs
- `domain` (required): apex domain (no protocol), e.g. `animeparadox.online`
- `verificationMethod` (optional, default `dns_txt`): `dns_txt | html_tag | html_file`
- `sitemapPath` (optional, default `/sitemap.xml`)

## Preconditions
- Operator can login to Google in OpenClaw browser (HITL if needed).
- If `dns_txt` verification is used, assistant must be able to place a TXT record in DNS (Cloudflare preferred).

## Resumable run layout
- `runs/gsc_<domain>_<YYYY-MM-DD>/status.json`
- `runs/gsc_<domain>_<YYYY-MM-DD>/artifacts/verification.json`

`status.json` includes step state and `needs_human` gates.

## Steps
1) `ensure_login`
   - Open GSC: `https://search.google.com/search-console`
   - If not logged in, set `needs_human` and ask user to login in the OpenClaw browser.

2) `add_property`
   - Prefer **Domain property** for full coverage (requires DNS TXT).
   - Property value: the apex domain.

3) `get_verification_token`
   - For DNS method, copy the TXT record name/value GSC provides.
   - Write `artifacts/verification.json` with: domain, method, txtName, txtValue, collectedAt.

4) `apply_dns`
   - Add TXT record in DNS provider (Cloudflare if available).
   - Wait for propagation best-effort; then click Verify.

5) `submit_sitemap`
   - Submit `https://<domain>/sitemap.xml`.

6) `send_telegram`
   - Send to Telegram chat id `6475983054`:
     - Domain
     - Verification method
     - Property URL in GSC (if visible)
     - Sitemap URL + submission status

## Failure handling
- If verification fails due to propagation, wait and retry; if still failing, set `needs_human` with next steps.
