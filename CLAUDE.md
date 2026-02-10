# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Air Quality Dashboard for the Qingping Air Monitor 2 — a single-page client-side web app (`index.html`) that visualizes air quality data. Hosted on GitHub Pages. No build system, no bundler, no package manager.

## Development

Open `index.html` directly in a browser or serve it locally:
```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

There are no tests, linters, or build steps.

## Architecture

Everything lives in a single `index.html` file (~2400 lines) containing inline CSS and JavaScript. No separate JS/CSS files.

**External dependencies (loaded via CDN):**
- SheetJS (xlsx.js) — client-side Excel file parsing
- Chart.js — interactive charting

**Two data sources:**
1. **Excel file upload** — parses `.xlsx` files from the Qingping device's export (auto-loads `example.xlsx` if present)
2. **Qingping OAuth API** — fetches live data directly from the device via `apis.cleargrass.com`. Uses OAuth2 client credentials flow (`oauth.cleargrass.com/oauth2/token`). Credentials stored in `localStorage`. Optional CORS proxy (`corsproxy.io`) for browser access.

**Key code sections (all within `<script>` in index.html):**
- `CONFIG` object — chart colors, air quality thresholds (EPA/ASHRAE/WHO-based), pagination settings
- Air quality helpers (`getMetricType`, `getAirQualityInfo`) — map metric values to Good/Moderate/Poor/Bad/Severe levels with color coding
- Date parsing (`parseDate`) — handles Excel serial dates, DD/MM/YYYY format, and standard date strings
- API integration (`authenticateQingping`, `getDevices`, `getDeviceHistory`, `syncFromDevice`) — full Qingping OAuth flow with paginated data fetching (200 records/batch)
- `processFile` — Excel parsing pipeline: read → extract headers → build data objects → initialize all views
- Chart rendering (`renderChart`, `renderAqChart`) — main multi-metric line chart + color-coded AQ detail chart
- Data table (`displayTable`) — paginated table with AQ color-coded cells

**Global state variables:** `allData`, `filteredData`, `headers`, `chart`, `aqChart`, `timeColumn`, `numericColumns`

## Reference Documents

- `api.pdf`, `docs.pdf`, `spec.pdf` — Qingping API documentation (not tracked in git)
- `example.xlsx` — sample data file from the device

## API Details

The Qingping API uses Cleargrass domains (Qingping's parent company):
- Auth: `oauth.cleargrass.com/oauth2/token` (Basic auth with app key:secret)
- Devices: `apis.cleargrass.com/v1/apis/devices`
- History: `apis.cleargrass.com/v1/apis/devices/data` (supports `mac`, `start_time`, `end_time`, `limit`, `offset`)

These are **OAuth API** credentials (not IoT API) — a common source of user confusion handled in the UI.
