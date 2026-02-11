# LinkHero Product Intake (Local MVP) — SPEC

## Summary
Create (or update) a LinkHero `Product` by scraping a public product landing page URL using the local `linkhero/mvp` app (Playwright), uploading assets to S3-compatible storage (Cloudflare R2), persisting the record into Postgres (Neon), and finally sending the resulting Product page URL to Telegram.

## Inputs
- `productUrl` (required): Full URL to the product landing page.
- `productSlug` (optional): Preferred product id/slug. If omitted, derive from URL hostname.
- `port` (optional, default `3001`): Local dev server port.

## Preconditions
- Local repo exists at `/Users/qianliu/Documents/Projects/linkhero`.
- `mvp/.env.local` contains at least:
  - `DATABASE_URL`
  - `STORAGE_REGION`, `STORAGE_BUCKET_NAME`, `STORAGE_ACCESS_KEY_ID`, `STORAGE_SECRET_ACCESS_KEY`, `STORAGE_ENDPOINT`, `STORAGE_PUBLIC_URL`
  - (optional) `OPENAI_API_KEY` for copy rewriting
- Playwright browsers installed: `npx playwright install chromium`.

## Behavior
### Recommended (CLI)
1. Ensure local `mvp` dependencies are installed.
2. Ensure Playwright Chromium is installed.
3. Run the local runner:
   - `cd /Users/qianliu/Documents/Projects/linkhero/mvp`
   - `npm run products:intake -- --url "<productUrl>" [--slug "<productSlug>"] --port 3001`
4. Parse stdout JSON to get `productId` and `prodUrl`.
5. Send a Telegram message containing `prodUrl`.

### Fallback (UI, HITL)
1. Start local MVP and use `/products/new` entry (section: “从 URL 自动抓取”) to trigger intake.
2. Confirm the app redirects to `/products/<productId>`.
3. Send a Telegram message containing the production Product URL.

## Outputs
- `productId`
- `productUrlNormalized`
- `productPageUrlLocal` (e.g. `http://localhost:3001/products/<productId>`)
- `productPageUrlProd` (e.g. `https://linkhero.online/products/<productId>`)

## Notes
- This skill is intended to run on **local MVP**, not production.
- Title cleanup is not automatic; user can request edits after viewing.
