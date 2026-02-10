# Air Quality Dashboard for Qingping Air Monitor 2

A beautiful, interactive web interface to visualize air quality monitoring data from Qingping Air Monitor 2 files. Built with pure HTML, CSS, and JavaScript - works completely client-side on GitHub Pages.

[Open the Air Quality Dashboard](https://janwalz.github.io/Air-Quality-Dashboard/)

## Features

- **Excel File Support**: Automatically loads the included Excel file or allows manual upload
- **Qingping API Integration**: Sync live data directly from your Qingping Air Monitor 2 device
- **Interactive Time-Series Line Chart**: Visualize data over time with line graphs
- **Date Range Filtering**: Filter data by date ranges with chart updates
- **Air Quality Color Coding**: Data is color-coded by air quality level (Good/Moderate/Poor/Bad/Severe)
- **CSV Export**: Export synced API data to CSV
- **100% Client-Side**: All processing happens in your browser

<img width="1290" height="1137" alt="image" src="https://github.com/user-attachments/assets/2c192905-249d-48c7-83ab-4c1fa4316fd7" />

## Qingping API Setup

The API integration lets you sync data directly from your device. It requires a local server because the Qingping API does not support CORS (browser cross-origin requests).

1. Register at [developer.qingping.co](https://developer.qingping.co/)
2. Add your device in the Qingping+ mobile app
3. Go to the [OAuth API Console](https://developer.qingping.co/main/oauthApi) (not IoT API)
4. Create an application to get your **App Key** and **App Secret**
5. Serve the dashboard locally:
   ```bash
   python3 -m http.server 8000
   # then open http://localhost:8000
   ```
6. Click "API Settings", enter your credentials, and sync

> **Note:** You can choose how much data to sync (7 days, 30 days, 90 days, 1 year, or all). The API connection only works from a local server. It will not work on GitHub Pages due to browser CORS restrictions. The Excel file upload works everywhere.

## Technical Details

- **Excel Parsing**: Uses [SheetJS](https://github.com/SheetJS/sheetjs) (xlsx.js) for client-side Excel file reading
- **Data Visualization**: Uses [Chart.js](https://www.chartjs.org/) for interactive, responsive charts
- **No Backend Required**: All data processing happens in the browser

## License

Free to use.
