# Flipkart Ads Monitor — Frontend

A single-file browser tool: upload (or auto-sync) daily Flipkart ad reports
and get a plain-English action plan for **sudden spikes and falls**, Campaign
and FSN wise, compared two ways:

- **Vs. yesterday** — the direct day-over-day check
- **Vs. the trailing 7-day average** — smooths out noise, catches slower drift

Runs entirely in your browser. Connecting a backend (see `../backend`) adds
automatic daily capture from Flipkart's scheduled report email — without one,
everything still works via manual upload.

## Tabs

1. **Today's action plan** — pick a date, see a plain-English verdict, then
   every unusual Campaign/FSN move with a suggested action. Toggle between
   "vs. yesterday" and "vs. 7-day average." Export to Excel.
2. **Upload & history** — add one day's FSN report at a time; see all saved
   days and their source (manual vs. automatic).
3. **Compare periods** — week vs. week, or month vs. month, overall and per
   campaign.
4. **Settings** — alert sensitivity thresholds, and the optional backend
   connection (URL + API key, with Test/Sync/Push controls).

## Publish on GitHub Pages

1. New public GitHub repo → upload `index.html`.
2. Settings → Pages → Deploy from branch `main` / root.
3. Live in ~1 minute at `https://<username>.github.io/<repo>/`.

## Verified

- 28 automated logic tests — run identically against this file's embedded
  logic and the backend's copy, confirming both are equivalent
- A full DOM simulation drove the actual UI: uploaded 10 days of synthetic
  data, confirmed spike/fall alerts fire correctly in both comparison modes,
  confirmed settings changes correctly suppress alerts below new thresholds
- A full-stack integration test connected this exact frontend code to a real,
  running instance of the backend: synced 8 real days, pushed a new day and
  confirmed it landed via a direct backend API read, and confirmed the
  backend's own independently-computed action plan agreed with the
  frontend's local computation on the same data
