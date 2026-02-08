# Local config (machine-specific, not part of the spec)

This folder is for **machine-local defaults** that multiple desktop agents can reuse.

- Do **not** put secrets here.
- Prefer stable entrypoints and URL patterns.

## Similarweb source of truth on this machine
1) Prefer collecting via **3ue Tools Share Similarweb PRO** (most consistent with our Semrush mirror workflow):
   - Home: https://sim.3ue.com/#/activation/home
   - Website performance URL pattern:
     `https://sim.3ue.com/#/digitalsuite/websiteanalysis/overview/website-performance/*/999/3m?webSource=Total&key=<domain>`
2) Fallback: cached/exported metrics in: `/Users/qianliu/Documents/Projects/035 mrrfast/data/similarweb_3m_metrics.csv`
