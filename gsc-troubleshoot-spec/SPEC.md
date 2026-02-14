# gsc-troubleshoot — SPEC

## Purpose
Make a site **GSC-healthy** (indexing + crawl + canonical + sitemap/robots + www/https + structured data), and create an evidence-backed troubleshooting report.

This skill is about **quality + correctness**, not growth strategy.

## Inputs
- domain or product shortName / project path
- optional: explicit GSC property id (e.g. `sc-domain:example.com`)

## Outputs (required)
- Repo fixes (safe + reversible) with commits when applicable
- `docs/GSC_TROUBLESHOOT_REPORT.md`
  - issues → root cause → fix (file path) → verification steps
- In GSC:
  - submit sitemap (if needed)
  - trigger **Validate fix** for relevant issue types

## Non-negotiables (Gate 0)
- Identify the repo + deployment platform (Cloudflare Pages / Vercel / etc.)
- Determine canonical base URL (https + apex vs www)
- Confirm `robots.txt` and `sitemap.xml` are accessible and consistent with canonical base

## Checklist (must run)
1) **Robots/Sitemap overrides**
- If Next.js app routes exist (`app/robots.*`, `app/sitemap.*`), ensure `public/robots.txt` / `public/sitemap.xml` do not override them.

2) **Canonical / metadataBase**
- Ensure canonical points to the intended host and is consistent across pages.

3) **www / apex health**
- `https://<apex>/` and `https://www.<apex>/` should not return 52x.
- Fix via DNS/Cloudflare Pages custom domains/SSL.

4) **Static export constraints**
- If `output: 'export'`: do not rely on middleware in production.
- Use platform redirects (`public/_redirects` for Cloudflare Pages).

5) **Structured data sanity**
- Check Enhancements (FAQ/Breadcrumb etc.) for invalid items.

6) **GSC validation actions**
- For issues like 404/5xx: click **Validate fix** and record “Started:” timestamp.

## Evidence
- For each class of fix, capture:
  - before (GSC issue example URL + status)
  - after (live URL headers + updated GSC validation state)

## Safety
- Do not delete GSC properties.
- Prefer adding Owners (shared ownership) over migration.
- Do not store passwords/secrets in repos, dossiers, or Obsidian.
