# FEC Analytics Platform — Case Study

> A real-time, multi-POS business intelligence platform I designed and built end-to-end for a **25+-outlet family-entertainment chain across India** — consolidating three point-of-sale systems into one source of truth with **25+ analytics modules**, AI assistance, and automated reporting.

<p>
  <img src="https://img.shields.io/badge/role-Sole%20Builder-orange" alt="Sole Builder">
  <img src="https://img.shields.io/badge/outlets-25%2B-blue" alt="25+ outlets">
  <img src="https://img.shields.io/badge/modules-25%2B-green" alt="25+ modules">
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black" alt="JavaScript">
  <img src="https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white" alt="Node.js">
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=flat&logo=supabase&logoColor=white" alt="Supabase">
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white" alt="PostgreSQL">
</p>

> 🔒 **Why this is a case study, not full source.** This platform runs on a real company's confidential operational data and proprietary POS integrations, so production code is kept private. This page documents the architecture, every analytics module, and the key engineering decisions. _Code walkthrough available on request._

---

## The problem

A chain of 25+ entertainment outlets across India ran on spreadsheets and disconnected point-of-sale systems — a wallet/arcade POS in some stores, a separate F&B POS in others, and a third system for activities, each with its own data shape and quirks. Leadership had **no single, trustworthy, real-time view** of the business.

Answering "how did we do last week?" meant a full day of manual Excel work. Different reports gave different numbers. There was no way to know if a bad month was "fewer customers" or "customers spending less." F&B revenue was off by ~2× for weeks before anyone caught it.

---

## What I built

A single web dashboard that pulls from every source, reconciles it, and turns it into decisions. Built entirely from scratch, solo.

---

## Architecture

```mermaid
flowchart LR
    A[Semnox / Parafait<br/>Arcade & Wallet POS] --> R[Serverless functions<br/>Node.js]
    B[PetPooja<br/>F&B POS] --> R
    C[Activity API<br/>internal] --> R
    R --> D[Reconciliation layer<br/>single revenue definition]
    D --> E[(Supabase / PostgreSQL<br/>historical store)]
    E --> F[GitHub Actions<br/>automated sync pipelines]
    D --> G[Dashboard SPA<br/>vanilla JS + charts]
    G --> H[Gemini AI assistant<br/>voice + text]
    G --> I[Brevo<br/>daily digest email]
```

- **Frontend:** vanilla JavaScript single-page app — dependency-light, fast, no build step.
- **Backend:** Node.js serverless functions (Netlify) fetching, caching, and reconciling three POS systems.
- **Data layer:** Supabase (PostgreSQL) — migrated from a caching setup that kept timing out; automated GitHub Actions pipelines keep historical data fresh.
- **AI:** Gemini LLM answering natural-language questions with live dashboard data as context (voice + text input).

---

## Every analytics module

### Revenue & Operations

| Module | What it shows |
|---|---|
| **Revenue & Total Sales** | Per outlet, per company, per day. Activity / F&B / combined toggle. Handles Jus Jumpin (FEC) and The Knockout (sports bar) as separate companies in one view. |
| **Footfall & Spend-per-Head** | Separates "more customers came" from "each customer spent more" — the two tell very different stories. |
| **Time-wise & Hourly Sales** | Hourly revenue breakdown — identifies exact peak windows per outlet. |
| **Staffing Optimizer** | Translates peak-hour data into recommended counter headcount per time slot. |
| **Voucher Analytics** | Denomination breakdown, usage rates, OTP funnel tracking (14-day and 21-day redemption cohorts), charm pricing impact. Pivot table: owner × store × month in one view. |
| **Walk-in Breakdown** | Walk-in revenue split by package tier (Unlimited / 120 / 90 / Extension) — real `wr` field backfilled to distinguish package revenue from socks addon money. |
| **Socks Sale Pattern** | Paid sock-session attach-rate by package and store. Counts only paid sessions (net >₹60), filters redemptions. |
| **Socks Ratio Maintenance** | Target vs actual attach-rate alert — flags underperforming stores in real time. |

### F&B Deep-Dive

| Module | What it shows |
|---|---|
| **F&B Compare** | Period A vs period B head-to-head across outlet / category / item / channel / payment method. Live dropdowns with multi-pick chips. |
| **Menu Engineering Quadrant** | Star / Plough-horse / Puzzle / Dog classification with editable food-cost % per item. |
| **F&B Attach Rate** | F&B revenue earned per guest beyond the activity ticket. |
| **Day-Part Sales** | Breakfast / lunch / dinner / late-night breakdown — shows where revenue concentration sits. |
| **Channel Economics** | Dine-in vs aggregator (Zomato / Swiggy) with configurable commission rates → true P&L per channel. |
| **Discount & Void Leakage** | Reconciled to PetPooja source exactly — quantifies revenue leakage from discounts and voided orders. |

### Customer Intelligence

| Module | What it shows |
|---|---|
| **Cohort & RFM Retention** | Which acquisition months retain customers vs which leak them out the bottom? Classic cohort triangle. |
| **Customer LTV** | Lifetime value per acquisition cohort — revenue accrued from first visit onward. |
| **Top Spenders** | Ranked client spend, filterable by date range and outlet. Identifies high-value customers for relationship marketing. |
| **Age Analysis** | Child / adult segmentation + age-band breakdown from 22,000+ customer DOBs via the internal customer API. Flags placeholder DOBs (Jan 1) automatically. |

### Predictive & Intelligence

| Module | What it shows |
|---|---|
| **Forecasting & Accuracy** | Expected revenue vs actual. Tracks how accurate previous forecasts were — builds trust in the model over time. |
| **Anomaly Detection** | Automated spike/dip alerts with deviation severity scores. Flags outliers without manual review. |
| **Weather-Sensitivity** | Pulls live Open-Meteo API data — correlates rainfall and temperature with footfall and revenue per outlet. |
| **Store Performance Prediction** | Forward projections with ML confidence bands — "based on trend, what should next week look like?" |
| **Targets & Pulse** | WoW / MoM / YoY deltas against editable RAG (Red/Amber/Green) targets per KPI — momentum at a glance. |

### Operations & Compliance

| Module | What it shows |
|---|---|
| **SOP & CCTV Performance** | Store compliance scores — checks whether standard operating procedures were followed, links deviations to revenue impact. |
| **Deviations & Operations Monitor** | Automated flag → revenue correlation. Shows the cost of operational deviation. |
| **Promotions Analysis** | Incentive programme ROI — did the promotion drive incremental revenue or just discount existing demand? |

### Platform Features

| Feature | Detail |
|---|---|
| **Live Geo Map** | All 25+ outlets plotted on Leaflet.js — click any pin for store detail. Knockout (sports bar) also on map. |
| **Store & State Rankings** | Full-list outlet rankings by revenue and footfall — national view to single-store drill-down in one table. |
| **Multi-company switch** | Jus Jumpin (FEC) and The Knockout (sports bar, separate POS) in one dashboard — switch at top level. |
| **Multi-POS** | Semnox/Parafait (arcade/wallet), PetPooja (F&B), and internal activity API — unified into a single revenue definition. |
| **AI Assistant** | Ask questions in plain English. Gemini LLM uses a live summary of the dashboard as context. Voice + text input. |
| **Global PDF/Excel export** | PDF via `window.print()` + `@media print` CSS suppression of nav. Excel via `.ms-excel` blob download — works on every tab. |
| **Daily Digest** | Automated 9am IST email to management via Brevo. Filter-aware — respects company and outlet filters. |

---

## Engineering highlights

### The F&B double-count that no one noticed for weeks

F&B revenue was reading ~2× too high across all outlets. Every dashboard said one number. PetPooja said another. Leadership didn't trust my reports.

Root cause: PetPooja's API, when asked for "orders on June 25th," quietly returned June 25th **plus** late-night orders whose timestamps had rolled over from June 24th. Every single day was being double-counted by the overnight session.

Fix: validate each order's own `order_date` against the day requested — ignore anything that doesn't match. One filter, applied consistently. Dashboard totals matched source systems **exactly** for the first time.

> This is the moment that rebuilt leadership trust in the numbers.

### Reconciling three completely different POS systems

Each POS system has its own data model, own definition of "a sale," and its own quirks:
- **Semnox/Parafait** (arcade/wallet): revenue = New Card + Recharge only. Activity transactions and bowling are card *usage*, not revenue. TransactionNetAmount inflates by ~2× if you use it naively.
- **PetPooja** (F&B): correct totals, but the date-rollover issue above. Also requires GET-with-body requests that `fetch()` can't handle — custom Node.js `https` module calls.
- **Activity API**: activity bookings, birthday parties, footfall source. Separate endpoint, different auth.

Building a single revenue definition that is *comparable* across all three systems took months of investigation and iteration.

### Migration from Netlify Blobs to Supabase/PostgreSQL

The original caching layer (Netlify Blobs) had a 10-second function timeout and kept failing on large date-range pulls. The fix wasn't "tune the cache" — it was migrate everything.

Supabase PostgreSQL now stores all historical data. GitHub Actions pipelines run scheduled syncs. New features ship in hours instead of fighting infrastructure. The database gives us the join queries and cross-outlet aggregations that were impossible before.

### Data integrity at scale

- F&B revenue reconciled to PetPooja source to the rupee
- Voucher incentive logic handles edge cases: owner-active per-store (not global), charm pricing, LEFT employee precedence
- Age analysis flags 18,000+ January-1 placeholder DOBs automatically
- Socks tracking filters ₹0 redemptions and sub-₹60 sessions that inflate attach rate

### Management reporting that runs on the clock

Leadership no longer asks for numbers; the numbers arrive. Every day, without anyone pressing a button:

- **"Sales so far today" flashes at 3 PM, 6 PM and 9 PM** for both companies. Each run triggers a fresh sync of the sales sources first, then holds until the hour, so the message lands *on* the hour with numbers that are minutes old, not hours.
- **A full daily brief at 11 PM** for the family-entertainment brand and **6 AM the next morning** for the late-night sports bar, each covering its own *closed* trading day, so the bar's report includes its after-midnight trade instead of cutting it off.

### When the scheduler itself was the bug

The syncs originally ran on GitHub Actions' built-in `schedule` trigger. It worked, until GitHub started delaying **every** scheduled run by around five hours. A "3 PM" sales flash arriving at 8 PM is worse than no flash.

The fix moved the *clock* out of GitHub entirely: a tiny Cloudflare Worker whose only job is to fire on a precise cron and dispatch the GitHub workflow. The work still runs in GitHub Actions; only the timing moved. Two constraints shaped the result: the free plan allows **5 cron triggers per account** (all five are now in use, so every schedule had to be consolidated deliberately), and live syncs run **only during trading hours**, which keeps Actions usage within budget instead of syncing an empty night every hour.

### A vendor changed their API overnight, and the sync reported "success"

One night the F&B POS provider moved its API behind a CDN. The CDN rejected the request style the integration had always used (a GET request carrying a JSON body) with an HTML 403 page. The sync step treated every non-2xx response as "no orders," so it kept reporting **success** while writing **₹0 F&B** for two days. That ₹0 flowed into the dashboard, the flashes and the daily brief.

The code fix was small: send the same body as POST. The real fix was the failure mode:

- **A rejection is never recorded as a ₹0 day.** Any response that isn't an explicit success now fails the outlet-day, fails the job, and writes an error to the sync log.
- **The sales flash now warns when a data source is stale**, so a broken feed surfaces in the very next report instead of in a manager's question two days later.
- The missing days were backfilled, recovering after-midnight orders that had been lost.

The lesson worth keeping: **a job that can't tell "no data" from "request refused" will eventually report a confident zero.** Silent success is more dangerous than a loud failure.

### The harder failure: data that's wrong without any error

The same week, a second source failed in a worse way. The events booking system sits behind a CDN that first stopped answering requests from the sync servers, then started answering them with **stale cached copies**: clean, valid data with no error at all, just missing the newest bookings. The same request from an office PC showed bookings the sync never received. Two morning reports went out without large events in them.

Retries alone can't fix this, because nothing fails. The fix had three layers:

- **Never accept a cached answer:** every request carries a cache-busting parameter and `no-cache` headers, with retries, timeouts and a hard failure if any chunk doesn't arrive.
- **Sync at the right moment:** late-night events are billed close to midnight, so the final sync before the morning report now includes the events source, not just sales.
- **Cross-check sources against each other.** Event food appears in the café's POS; the event itself is a booking in a separate system. **Event food sold on a day with no event booking recorded** means a booking is missing, which is exactly the signal that exposed this bug.

Every daily brief now opens with a **data check**: an amber box above the figures if any source failed or is older than the trading-day close, or if the cross-check finds that mismatch, and a green "all data sources up to date" line otherwise. **The report tells management when not to trust it**, instead of management finding out from a store.

### Revenue definitions are rules, and rules must match the books

Getting café revenue to match each store's own daily total to the rupee meant settling definitions explicitly, not leaving them to whatever the code happened to do:

- **Service charge stays in** café revenue, because the store counts it.
- **Event food is taken out at its share of the bill *including* tax.** Removing the pre-tax item amount had left the tax portion inside café revenue.
- **Card sales are net of refunds**, with each refund deducted on its own day and hour; historical days were corrected by migration.
- **Exclusions match on item name, not just category.** One outlet rang up bottled water under "Refreshers" and "Open Item," so a category-only rule had excluded *nothing* for that outlet's entire history. Matching item names fixed it and corrected the history.

---

## Impact

- Leadership opens a **browser tab** instead of waiting a day for spreadsheet work.
- Revenue numbers **match source POS systems** — reports are trusted.
- 25+ analytics modules from a **single, self-taught, one-person build**.
- What took a day of Excel work now takes **seconds**.
- Anomaly detection flags issues **before** the weekly review meeting.

---

## Tech stack

`JavaScript` · `Node.js` · `Serverless (Netlify Functions)` · `Supabase / PostgreSQL` · `GitHub Actions` · `Cloudflare Workers (cron)` · `PetPooja API` · `Semnox / Parafait API` · `Gemini LLM API` · `Leaflet.js` · `Chart.js` · `Brevo API` · `Data Visualization` · `REST APIs` · `Git`

---

### Author

**Souvik Kundu** — Business Intelligence & Automation Engineer.
Designed and built this platform entirely solo: data integration, reconciliation engine, backend, 25+ analytics modules, AI assistant, and deployment.

📫 [LinkedIn](https://linkedin.com/in/souvik-kundu-bi) · [GitHub](https://github.com/Souvikkundu369)
