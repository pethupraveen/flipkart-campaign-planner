# Flipkart Ads Monitor — Backend

Reads Flipkart's daily 10am scheduled report email automatically, stores it,
and detects sudden spikes/falls — both **vs. yesterday directly** and
**vs. the trailing 7-day average** — per Campaign and per FSN.

## Verified

- `npm test` — 28 automated tests on the detection/parsing logic
- Actually booted this server, seeded it with real spreadsheet uploads via its
  own API, and confirmed the `/api/action-plan/:date` endpoint returns
  correct spike detection (220% spend jump, -67% ROAS fall, classified as a
  risk) — not just code review
- The frontend (`../frontend/index.html`) carries an identical copy of this
  logic and was verified against a live instance of this backend: it
  connected, synced days, pushed a new day, and the two independent
  computations agreed on the result

## 1. Set up the mailbox

1. Note the email address that receives Flipkart's scheduled report (from
   Seller Hub → Advertising → your scheduled report's recipient field).
2. Generate an **app password** for that mailbox (not your normal password):
   - Gmail/Workspace: Google Account → Security → 2-Step Verification → App
     passwords
   - Outlook/365: account security → App passwords
   - Zoho: Settings → Security → App Passwords
3. Confirm IMAP is enabled for that mailbox.

## 2. Configure

```bash
cp .env.example .env
```
Fill in `IMAP_HOST`/`PORT`, `IMAP_USER`, `IMAP_PASS`, `REPORT_SUBJECT_MATCH`
(copy a distinctive phrase from an actual report email's subject line), and
make up a long `API_KEY`.

## 3. Test locally

```bash
npm install
npm test              # logic tests — should show "0 failed"
npm run fetch-now      # tries fetching today's email right now
npm start               # starts the API + cron
```

## 4. Deploy (Render.com, ~10 min, free tier)

1. Push this folder to a GitHub repo.
2. Render → New → Web Service → connect the repo. Build: `npm install`.
   Start: `npm start`.
3. Add every `.env` variable under Render's Environment tab (Render doesn't
   read `.env` files directly).
4. Add a persistent disk (mount at `/data`), set `DB_PATH=/data/data.sqlite`
   — otherwise your data resets on every deploy.
5. Deploy, then `curl https://your-service.onrender.com/api/health`.

Free tier sleeps when idle, which can delay the 10:15am cron — either use
Render's paid tier or an external pinger (e.g. UptimeRobot) hitting
`/api/health` every few minutes.

## 5. Connect the frontend

In the frontend, go to **Settings → Live backend**, paste the URL and API
key, press **Test connection** then **Sync days now**. From then on, manual
uploads in the frontend also auto-push to this backend.

## API reference

All endpoints except `/api/health` require header `x-api-key: <API_KEY>`.

| Method | Path | Purpose |
|---|---|---|
| GET | `/api/health` | liveness check, no auth |
| GET | `/api/days` | all saved dates + snapshots |
| GET | `/api/days/:date` | one day's snapshot |
| DELETE | `/api/days` | wipe all stored days |
| POST | `/api/days/:date/upload` | manual upload (multipart `file`) |
| POST | `/api/days/:date/snapshot` | push an already-parsed snapshot (JSON) |
| GET | `/api/action-plan/:date?spendPct=&roasPct=&ctrPct=&cvrPct=&minSpend=&window=` | **the main endpoint** — full spike/fall action plan for one date, both vs-yesterday and vs-trailing-average |
| GET | `/api/compare?aStart=&aEnd=&bStart=&bEnd=` | compare two date ranges (week or month) |
| GET | `/api/ingest/log?limit=30` | recent email-fetch attempts |
| POST | `/api/ingest/run-now` | trigger an email fetch immediately |

## Files

- `src/logic.js` — all analysis rules (parsing, snapshots, spike/fall
  detection both modes, period comparison). Mirrored exactly in the
  frontend's `index.html` — change both together, `npm test` both.
- `src/logic.test.js` — 28 tests for the above
- `src/db.js` — SQLite storage
- `src/emailFetcher.js` — IMAP connection + attachment parsing
- `src/fetchOnce.js` — CLI for `npm run fetch-now`
- `src/server.js` — Express API + cron
