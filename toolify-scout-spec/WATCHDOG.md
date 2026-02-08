# WATCHDOG.md (toolify-scout-spec v0.1)

## Goal
Detect and fail fast on:
- selector/parser breakage (Toolify/Similarweb/Semrush HTML changes)
- rate-limit or CAPTCHA conditions
- output regressions (empty reports)

## Health checks (minimum)
1. **Toolify extraction sanity**
   - At least `min_toolify_items` (default 100) candidates extracted.
   - At least `min_unique_domains` (default 50) after dedup.

2. **Validation coverage**
   - Similarweb fetch success rate >= 50% for attempted validations.
   - Semrush fetch success rate >= 50%.
   - If both fall below threshold, mark run as degraded.

3. **Report non-empty**
   - Weekly top list length >= `min_top_k` (default 10), else fail.

4. **Time bounds**
   - Max runtime (soft): 30 minutes
   - Max runtime (hard): 60 minutes

## Degraded mode behavior
If validation sources are partially down:
- Still produce a report, but:
  - label it **DEGRADED**
  - restrict to candidates with at least one validated signal
  - include a “needs re-check” watchlist bucket

## Logging requirements
- Emit a compact run summary:
  - candidates extracted
  - unique domains
  - validations attempted/succeeded per source
  - thresholds used
  - top_k produced

## Safe abort conditions
Abort run when:
- persistent CAPTCHA pages detected
- HTTP 429 rate limit for > N minutes
- output would be empty
