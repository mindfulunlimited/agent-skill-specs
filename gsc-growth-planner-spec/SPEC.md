# gsc-growth-planner — SPEC

## Purpose
Use **GSC Performance + GA + external SEO tools (Semrush/Similarweb via portal)** to produce a concrete growth plan for a **content site monetized by ads / affiliate**:
- target keyword clusters (topic-first, not random keywords)
- page map (hub/pillar + cluster pages)
- internal linking plan
- lightweight content modules/features that improve rankings, CTR, and monetizable engagement

This skill is about **planning + execution backlog**, not troubleshooting.

## Inputs
- domain / product
- market: Global or specific country (default: Global; rerun for US if US-focused)
- competitors: optional seed list (3–5). Skill must also discover **SERP competitors** from keyword tools.

## External tools constraint
- Semrush/Similarweb must be accessed via the **3ue portal** (not official sites).
- Credentials must NOT be stored in repos. Use centralized ops account note:
  - `~/Documents/Operations/Accounts/SEO_Tools_3ue.md`

## Outputs (required)
- `docs/KEYWORD_RESEARCH_EXTERNAL.md`
  - Similarweb + Semrush findings + export notes
  - SERP competitor expansion notes (keyword → competitor → keyword loop)
- `docs/KEYWORD_OPPORTUNITIES.csv`
  - keyword, intent, volume (if available), KD, current position band, recommended page URL, priority
- `docs/GSC_GROWTH_PLAN.md`
  - monetization model assumptions (ads/affiliate)
  - opportunity clusters (topic-first)
  - page map
  - internal linking plan
  - feature/modules plan (schema, related blocks, comparison tables, filters)
  - measurement plan (GA4 engagement proxies + optional outbound click tracking)
- `docs/CODING_AGENT_GSC_GROWTH_START.md`
  - paste-ready implementation prompt for coding agent (Gate0 + TODO + acceptance)

## Method
### 1) From GSC Performance (your truth)
- Queries with high impressions + position 8–20 (quick wins)
- Queries with high impressions + low CTR (snippet/title/intent mismatch)
- Pages ranking for multiple query clusters (cannibalization + hub opportunities)

### 2) From external tools (market expansion)
- Similarweb (via 3ue): top keywords, top pages, competitors
- Semrush (via 3ue): organic keywords/traffic/cost + paid keywords/paid traffic (even if 0) + top pages + top keywords + ref domains

### 3) SERP competitor expansion loop (must do 1–2 rounds)
- Seed keywords → find top ranking domains / competitors
- Pick top 5–10 SERP competitors → pull their top keywords/pages
- Merge into clusters; stop when incremental new high-quality clusters drops (<20%)

### 4) Plan synthesis rules
- Prefer **topic clusters** over random keyword pages.
- Every new/updated page must have:
  - target query group
  - unique intent
  - internal links (from hub + from at least 2 related pages)
  - relevant schema/modules (when appropriate)

## Measurement (GA4)
Default for ad/affiliate content sites (no ecommerce needed):
- Use engagement proxies for prioritization:
  - engaged sessions, engagement rate, avg engagement time, pages/session
- Optional (recommended) lightweight events:
  - outbound_click (by destination domain)
  - scroll_75
  - time_on_page_60s

## Safety
- No credential storage.
- Avoid spammy auto-generated pages without unique intent/value.
