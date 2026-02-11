# linkhero-product-intake-spec

Create a Product in LinkHero from a URL using the local MVP (Playwright scrape → upload assets to R2 → write to Postgres), then send the resulting Product URL to Telegram.

- Primary use: fast intake so the product exists in LinkHero for submissions.
- Does **not** write to Obsidian.

See `SPEC.md` for full contract.
