# gsc-growth-planner — SPEC

## Purpose
Use **GSC Performance + GA + external SEO tools (Semrush/Similarweb via portal)** to produce a concrete growth plan:
- target keyword clusters
- page map (pillar/hub + cluster pages)
- internal linking plan
- lightweight product features/modules that improve rankings & CTR

This skill is about **planning + execution backlog**, not troubleshooting.

## Inputs
- domain / product
- market: Global or specific country (default: Global; rerun for US if US-focused)
- optional: competitors list (3–5 domains)

## External tools constraint
- Semrush/Similarweb must be accessed via the **3ue portal** (not official sites).
- Credentials must NOT be stored in repos. Use centralized ops account note:
  - `~/Documents/Operations/Accounts/SEO_Tools_3ue.md`

## Outputs (required)
- `docs/KEYWORD_RESEARCH_EXTERNAL.md`
  - Similarweb + Semrush findings + export notes
- `docs/KEYWORD_OPPORTUNITIES.csv`
  - keyword, intent, volume (if available), KD, current position band, recommended page URL, priority
- `docs/GSC_GROWTH_PLAN.md`
  - opportunity clusters
  - page map
  - internal linking plan
  - feature/modules plan (schema, related content blocks, comparison tables, filters, etc.)
- `docs/CODING_AGENT_GSC_GROWTH_START.md`
  - paste-ready implementation prompt for coding agent (Gate0 + TODO + acceptance)

## Method
1) From GSC Performance:
- Queries with high impressions + position 8–20
- Queries with high impressions + low CTR
- Pages ranking for multiple queries (cannibalization opportunities)

2) From external tools:
- Similarweb: top keywords, top pages, competitors
- Semrush: organic keywords/traffic/cost + top pages + ref domains

3) Plan synthesis rules
- Prefer **topic clusters** over random keyword pages.
- Every new page must have:
  - target query group
  - unique intent
  - internal links (from hub + from at least 2 related pages)
  - relevant schema (when appropriate)

## Safety
- No credential storage.
- Avoid spammy auto-generated pages without unique intent/value.
