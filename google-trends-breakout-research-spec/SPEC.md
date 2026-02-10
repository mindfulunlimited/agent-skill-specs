# Google Trends Breakout 新词调研 Spec (v0)

## Goal
Given a batch of seed keywords, use Google Trends to:

1) **Discovery**: For each seed keyword group (5 seeds per page), collect **Related queries/topics** that are marked **"Breakout"**.
2) **Comparison**: Take collected breakout terms, compare in Trends in groups of 5: **4 breakout terms + 1 anchor term "GPTs"** (or user-provided anchor), to approximate relative search interest.
3) **Filtering**: Drop terms that are clearly **person names / place names / sports events** (not suitable for “新词新站”).

## Inputs
- `keywords[]` (required): list of seed keywords.
  - If user provides multiline text, parse **by line first**; split each line by commas; do not concatenate across line breaks.
- Optional settings (defaults if omitted):
  - `geo`: `Worldwide`
  - `timeRange`: `past 7 days`
  - `category`: `All categories`
  - `searchType`: `Web Search`
  - `anchorTerm`: `GPTs`

## Outputs
A structured result + human-readable summary.

- `artifacts/breakout_terms.json`:
  - seedGroup → breakoutTerms[] with source seed + type(query/topic) + rank/label
- `artifacts/compare_results.json`:
  - comparisonGroup (4 terms + anchor) → notes on relative interest (screenshot refs optional)
- `report.md` (optional): bullets with:
  - breakout candidates (filtered)
  - excluded terms (with reason)
  - comparison highlights (which terms show meaningful interest vs anchor)

## Resumability
Persist state:
- `runs/trends_<YYYY-MM-DD>_<slug>/status.json`
- artifacts folder per run

Steps (one per tick):
- ensure_login
- collect_breakout (iterate seed groups of 5)
- filter_terms
- compare_with_anchor (iterate groups of 4 + anchor)
- finalize

Step states: `pending|running|done|needs_human|error`.

## Human-in-the-loop gates
Set `needs_human` if:
- Not logged in to Google
- Captcha / unusual traffic
- UI changed / elements cannot be found reliably

## Non-goals
- No ads keyword volume estimates
- No external scraping beyond Google Trends UI

## Execution guidance (important)
- **Not recommended to run as a fully autonomous subagent.** Google Trends Explore is UI-automation heavy and frequently triggers login/verification/UX drift.
- Preferred mode: **main session + human-in-the-loop (HITL)**.
  - The run state machine supports `needs_human` for login/captcha/permission prompts.
- Automation pacing: **1–3s between UI actions** to reduce throttling / unusual-traffic blocks.
