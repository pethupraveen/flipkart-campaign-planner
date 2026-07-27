# Flipkart Ads Optimizer — Weekly Action Plan

A free, single-file web tool for Flipkart sellers: upload your three weekly ad reports and get a bid-level optimization action plan, exportable to Excel.

**100% client-side.** Reports are parsed in your browser and never uploaded anywhere. Data persists in localStorage so you can return each week.

## Weekly workflow
1. Download from Flipkart Ads Manager: (1) Consolidated FSN report, (2) Consolidated daily report, (3) Placement performance report.
2. Open the tool → pick the week → drop the 3 files. Report types are auto-verified — a file dropped in the wrong slot is moved to the correct one automatically.
3. Press **Generate action plan**. Filter by campaign (multi-select with search), FSN/product search, or action type.
4. **Export to Excel** (Summary, Campaign Summary, FSN Actions, Placement Actions sheets) and apply changes in Ads Manager.

## Optimizer rules (priority order)
1. **PAUSE** — meaningful spend & clicks, zero units → price/stock/rating problem
2. **CUT BID** — ROAS below break-even → losing money
3. **SCALE** — ROAS ≥ target × multiplier → raise bids/budget
4. **REDUCE** — profitable but under target → trim bid 10–15%
5. **FIX CONTENT** — high views, very low CTR → listing is the bottleneck

All thresholds (min spend, clicks, views, CTR, scale multiplier, target/break-even ROAS) are tunable in Rules & thresholds. Optional monthly budget enables week-by-week spend pacing in the header strip.

## Publish on GitHub Pages
1. Create a public repo → upload `index.html` + `README.md`.
2. Settings → Pages → Deploy from branch `main` / root → Save.
3. Live in ~1 minute at `https://<username>.github.io/<repo>/`.

## Notes
- Break-even ROAS ≈ 1 ÷ profit margin (33% margin → 3.0).
- CVR basis: units ÷ clicks. Same FSN in different campaigns is analyzed per campaign.
- localStorage is per-browser/per-device; export Excel for permanent records.
