# KSERC-MIS

A browser-based Management Information System for monitoring Kerala's power system data. The dashboard brings daily demand, generation, hydro, power purchase and surrender, Indian Energy Exchange (IEX) prices, and load-curve information into one shared interface.

## Overview

KSERC-MIS is a single-page web dashboard built with HTML, CSS, and JavaScript. It reads operational workbooks and daily reports, processes their data in the browser, and synchronizes the results through Supabase.

The dashboard supports public viewing and authenticated data uploads. Viewers can explore published information without signing in. Authorized uploaders can import source files and update the shared dataset.

## Dashboard Sections

- **Summary:** Key figures for peak demand, energy consumption, hydro generation, purchase, surrender, and average IEX DAM and RTM prices. Includes daily consumption and hydro, purchase, and surrender charts.
- **Demand:** Daily highest, average, and lowest demand. Includes half-hourly demand profiles and comparisons with KSEBL projections when projection data is available.
- **Hydro · Purchase · Surrender:** Hydro generation statistics, purchase and surrender trends, daily tables, station-wise hydro data from Statistics reports, and time-window filters.
- **IEX Prices:** Daily DAM and RTM average prices, 15-minute price profiles, and month-wise price comparisons.
- **Load Curves:** Daily load-curve view generated from SLDC PDF reports.
- **Data:** Import coverage, data backup and restore, Excel export, and server re-sync. This section is available to authorized uploaders.

Most dashboard sections support monthly, all-months, and month-comparison views. The Hydro section also supports time windows.

### Backend Configuration

The application connects to Supabase from `index.html`. The current source contains the Supabase project URL and a publishable client key.

To connect a separate deployment:

1. Create or select a Supabase project.
2. Configure the database tables and access policies expected by the application.
3. Set the Supabase project URL and publishable key in the cloud-layer configuration in `index.html`.
4. Configure Supabase Authentication and the uploader access records.
5. Test public read access and authorized upload access before sharing the dashboard.

The repository archive does not include database migration files or SQL definitions for these tables and policies. Set up the backend schema and Row Level Security policies to match the application's read and write operations before deploying a new instance.

The browser-side key is a publishable key, not a secret service-role key. Keep database permissions enforced through Supabase Row Level Security. Never place a service-role key or other private credentials in frontend code.

## Importing Data

1. Sign in with an authorized uploader account.
2. Open the **Import** control.
3. Select one or more supported source files, or select a ZIP archive.
4. Review the import log for successful imports and parsing errors.
5. The application saves accepted records to the shared Supabase database.
6. Other dashboard viewers receive updates through the dashboard's periodic synchronization.

Previously imported non-PDF files are skipped using their import record. A changed file or a newer workbook version is processed again. The import log reports files that were accepted, skipped, or failed.

Viewers do not need to sign in to view published dashboard data. Only authorized uploaders have access to import and data-maintenance controls.

## Data Storage and Synchronization

- **Supabase** stores shared dashboard data.
- **IndexedDB** stores a local cache to speed up loading and retain the last downloaded data for offline viewing.
- The dashboard synchronizes updated records from Supabase and periodically checks for new data.
- Upload operations are restricted to authorized uploaders.
- The Data section provides JSON backup, backup restore for uploaders, Excel export, and manual re-sync.

## Technologies and Libraries

- HTML5
- CSS
- JavaScript
- Supabase JavaScript client
- SheetJS (`xlsx`) for Excel workbook parsing and export
- Chart.js for charts
- PDF.js for PDF text extraction
- JSZip for ZIP archive processing
- IndexedDB for local browser caching

The application loads its JavaScript libraries from CDN links in `index.html`.

## Repository Structure

```text
KSERC-MIS-main/
├── index.html   # Dashboard interface, styles, parsing, charts, sync, and import logic
└── README.md    # Project documentation
```

## Support and References

- [Supabase Documentation](https://supabase.com/docs)
- [Chart.js Documentation](https://www.chartjs.org/docs/)
- [SheetJS Documentation](https://docs.sheetjs.com/)
- [PDF.js Documentation](https://mozilla.github.io/pdf.js/)
- [JSZip Documentation](https://stuk.github.io/jszip/)
