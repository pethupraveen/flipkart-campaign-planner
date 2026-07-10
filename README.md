# Flipkart Campaign Planner & Optimizer

A free, single-file web tool for Flipkart sellers to plan monthly ad campaigns and turn weekly ad reports into a concrete optimization action plan — exportable to Excel.

**100% client-side.** Your reports are parsed in your browser and never uploaded anywhere. Data is saved in your browser's localStorage so you can return each week.

## What it does

**1 · Campaign planner**
- Set monthly budget, target ROAS and break-even ROAS
- Build a campaign list (PLA / PCA) with daily budgets, duration and per-campaign targets
- Live budget check (over/under allocation), expected revenue and units
- One-click suggested plan for a mobile-accessories catalog
- Export the plan to Excel

**2 · Weekly reports**
Upload three reports from Flipkart Ads Manager each week (.xlsx or .csv):
1. Consolidated FSN report
2. Consolidated daily report
3. Placement performance report

Column headers are auto-detected (fuzzy matching), so small naming differences between report versions are handled.

**3 · Action plan (rules-based optimizer)**
- **PAUSE / FIX LISTING** — spend + clicks but zero units → price / stock / rating problem
- **CUT BID** — ROAS below break-even → losing money
- **REDUCE** — profitable but below target ROAS → trim bid 10–15%
- **SCALE** — ROAS well above target → raise bid/budget 15–25%
- **FIX CONTENT** — high views, very low CTR → thumbnail/title/price is the bottleneck
- Placement-level budget-shift recommendations
- Weekly spend pacing vs your monthly budget
- Export the whole action plan to a multi-sheet Excel file

**4 · Rules & thresholds** — tune minimum spend, clicks, views, CTR and scale multiplier to your margins.

## Publish on GitHub Pages (free)

1. Create a GitHub account (github.com) if you don't have one.
2. Click **New repository** → name it e.g. `flipkart-campaign-planner` → Public → Create.
3. Click **uploading an existing file** → drag in `index.html` and `README.md` → Commit changes.
4. Go to **Settings → Pages** (left sidebar).
5. Under **Source**, choose **Deploy from a branch**, branch `main`, folder `/ (root)` → Save.
6. Wait ~1 minute. Your tool is live at:
   `https://<your-username>.github.io/flipkart-campaign-planner/`

To update later, just upload a new `index.html` over the old one.

## Weekly workflow

1. Monday: download last week's 3 reports from Flipkart Ads Manager.
2. Open the tool → tab 2 → pick the week → drop the 3 files.
3. Press **Generate action plan** → review → **Export to Excel**.
4. Apply the bid/pause/scale changes in Ads Manager.
5. Repeat next week — the pacing strip shows whether you're on budget for the month.

## Notes

- Break-even ROAS ≈ 1 ÷ profit margin (33% margin → break-even 3.0).
- localStorage is per-browser, per-device. Export to Excel for a permanent record.
- No dependencies to install; SheetJS and fonts load from public CDNs.
