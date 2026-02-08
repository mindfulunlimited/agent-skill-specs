# toolify-scout-spec (v0.1)

## Purpose
Discover fast-rising, promising, **not-too-new** AI tools/products listed on Toolify at scale, and output a **weekly candidate list** (domains) with rationale + links.

This spec is **agent-agnostic**: it defines inputs/outputs, state, and hard rules. An adapter (e.g., OpenClaw skill) can implement it.

## Key idea
Toolify provides broad coverage (thousands of items). We use Toolify pages as a **candidate generator** and then validate momentum using 3rd‑party traffic/SEO signals:
- Similarweb via `https://sim.3ue.com/…`
- Semrush via `https://sem.3ue.com/…`

We combine:
1) **Sampling** across Toolify’s corpus (to scale)
2) **Dedup + normalization** to domains
3) **Momentum validation** using Similarweb/Semrush thresholds
4) **Ranking** + **filters** to focus on “promising but not too new”

## Inputs
No user-provided seed list.

Required external sources (public web pages):
- Toolify listing/search/category pages
- Similarweb proxy pages (sim.3ue.com)
- Semrush proxy pages (sem.3ue.com)

Optional:
- Brave Search API to resolve domains when Toolify link is indirect or for supplemental signals.

## Outputs
Each run produces:
1) `weekly-report.md` (human-readable)
2) `weekly-candidates.json` (machine-readable)
3) raw snapshots (optional) used for debugging/audit

### Candidate object schema (minimum)
```json
{
  "domain": "example.com",
  "toolify": {
    "name": "Example",
    "url": "https://www.toolify.ai/tool/example",
    "categories": ["Writing"],
    "pricing": "Freemium",
    "first_seen_utc": "2026-02-07T…Z"
  },
  "signals": {
    "similarweb": {
      "url": "https://sim.3ue.com/website/example.com",
      "visits_last_month": 123456,
      "growth_3m_pct": 42.1
    },
    "semrush": {
      "url": "https://sem.3ue.com/analytics/overview/?q=example.com",
      "organic_traffic": 98765,
      "organic_growth_3m_pct": 35.0
    }
  },
  "score": 0.0,
  "rationale": [
    "3‑month visits growth +42% (Similarweb)",
    "Organic traffic growth +35% (Semrush)",
    "Not new: first seen > 45 days ago"
  ],
  "flags": ["passed_all_thresholds"]
}
```

## Pipeline (V1)
### Step A — Candidate generation (Toolify)
1. Enumerate Toolify listing pages to cover broad corpus.
2. Use **stratified sampling** across:
   - categories
   - “new”/“popular”/“trending” sections (if present)
   - alphabetical / paginated lists
3. Collect tool detail URLs and outbound website domains.

**Sampling goal:** produce ~1,000–5,000 raw candidates/week without crawling the entire site.

### Step B — Normalize & deduplicate
- Resolve and normalize to registrable domain:
  - remove protocol, `www.`, tracking params
  - canonicalize to `example.com`
- Deduplicate across Toolify entries (some tools appear multiple times)

### Step C — Not-too-new filter
We want **promising but not brand new**.

Accept if any of:
- Domain age (if available via signals) suggests > N days
- In run-state, first_seen date is older than `min_age_days` (default 45)
- Toolify listing age (if present) older than N days

Reject if:
- first_seen < `min_age_days`

### Step D — Validation (Similarweb + Semrush)
For each domain, fetch both sources (best-effort; partial acceptance allowed only in “review” bucket).

Compute growth over a recent window (3 months if available):
- Similarweb: visits + growth%
- Semrush: organic traffic + growth%

### Step E — Rank
Compute a composite score:
- Momentum (growth%) weighted more than absolute size
- Size floor (avoid tiny sites with noisy growth)
- Penalize adult/gambling/malware categories if detected

### Step F — Output weekly list
- Top `K` (default 30) + “watchlist” (next 50)
- Provide links to Toolify + Similarweb proxy + Semrush proxy
- Save artifacts and update run-state.

## Hard rules (non-negotiable)
1. **No user-provided list** required; discovery must be autonomous.
2. **Domain-centric output**: final candidates are unique domains.
3. Must produce weekly artifacts:
   - markdown report
   - json candidates
4. Must record state so “not-too-new” can be enforced.
5. Respect rate limits; avoid full-site crawling.

## Assumptions & limitations
- Toolify structure may change; extraction uses heuristics and is brittle.
- `sim.3ue.com` and `sem.3ue.com` are third-party proxies; availability and HTML structure may vary.
- Brave Search API has QPS/quotas; use it sparingly (only as fallback).
- Traffic metrics can disagree; the pipeline treats them as noisy estimates.
- Some tools use subdomains/app domains; normalization may collapse distinct products.

## Default parameters (suggested)
- `min_age_days`: 45
- `max_age_days`: 3650 (ignore very mature incumbents if desired)
- `sample_pages_per_category`: 10
- `max_candidates_per_run`: 2500
- Similarweb thresholds:
  - `visits_last_month >= 20000`
  - `growth_3m_pct >= 25`
- Semrush thresholds:
  - `organic_traffic >= 10000`
  - `organic_growth_3m_pct >= 20`
- Output:
  - `top_k = 30`, `watchlist_k = 50`
