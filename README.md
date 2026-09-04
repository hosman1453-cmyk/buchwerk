# BuchWerk

### A browser-based bookkeeping workspace for small practices

BuchWerk brings income, expenses, receipts, PDF documents, employee records, and working hours into a single multilingual interface. Built with vanilla HTML, CSS, and JavaScript, it explores how a practical administrative workflow can be delivered without a custom backend or a build pipeline.

**Languages:** Deutsch · English · Türkçe · Nederlands  
**Status:** Portfolio/demo project — selected workflows manually verified; production readiness is not established.

## What you can do

- **Understand the numbers:** annual income, expenses, profit/loss, personnel costs, monthly charts, and category breakdowns.
- **Manage transactions:** record and edit income and expenses, organize categories, and manage recurring expenses and private/cash-paid expenses.
- **Keep documents connected:** browse receipt images in a gallery/lightbox and manage a PDF archive.
- **Organize the team:** maintain employee information, contractual weekly hours, and target/planned/actual working-hour records.
- **Export reports:** use the Export Center for Excel, PDF, and JSON reports.
- **Back up the workspace:** export a portable Full Backup containing accounting data, receipt images, and PDF binaries; preview a compatible backup before restoring it.
- **Find older records:** year selectors include years found in income and expense records.
- **Try assisted receipt entry:** optionally use Google Gemini to extract document information. Always verify extracted dates, amounts, and categories before saving.

## Run locally

Save the application as `index.html`. No npm installation or compilation is required.

With Python 3 installed, open a terminal in the directory containing **only the files you intend to serve**:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

On Windows:

```powershell
py -m http.server 8000 --bind 127.0.0.1
```

Open [http://127.0.0.1:8000/index.html](http://127.0.0.1:8000/index.html) and leave the terminal running. Stop the server with `Ctrl+C`.

Use HTTP rather than double-clicking the HTML file: browser restrictions on `file://` can interfere with application behavior. Python serves the static file; it is not an accounting backend.

For a first evaluation, use a separate browser profile and fictional records. Internet access is needed for externally hosted libraries/fonts and optional AI analysis; this is not a fully offline application.

## How storage works

| Layer | Purpose |
| --- | --- |
| `localStorage` | Lightweight accounting metadata under `demo-pb-*` keys and interface preferences |
| IndexedDB: `PraxisBuchDemoDB`, version 3 | Browser-side document storage and restore journal |
| `pdfFiles` store | PDF binary records |
| `receiptImages` store | Receipt image binary records |
| `restoreJournal` store | Persistent information used by interrupted-restore recovery logic |
| Full Backup JSON | Portable `buchwerk-full-backup`, `schemaVersion: 1`, including embedded documents |

Receipt images are kept outside the expense JSON in localStorage to reduce quota pressure. The app loads them for display and reconstructs their portable representation when exporting a Full Backup. Legacy PDF compatibility code is also present.

Data belongs to the browser profile and origin, not to the HTML file. Changing browser, hostname, port, or moving from localhost to a hosted page does **not** automatically transfer records. Clearing site data can remove records and documents. Keep separate downloaded backups.

Restore uses validation, operation guards, and a persistent journal to coordinate metadata and document changes. These mechanisms are not a guarantee of atomic transactions across localStorage and IndexedDB.

## Backup and restore

1. Open **Berichte → Backup & Restore**.
2. Choose **Vollständiges Backup herunterladen** to download a complete backup. Store it outside the repository.
3. To restore, select a compatible Full Backup and review its preview counts before confirming.
4. After restore, verify the selected year, totals, receipt openings, and PDF downloads; then save a fresh Full Backup.

**Restore replaces the applicable data in the current workspace; it is not a merge operation.** Back up the current workspace first.

Report JSON exports and raw legacy localStorage backups are not Full Backups and are not accepted by this restore workflow. Legacy conversion requires a separate, reviewed migration process. Matching record counts alone does not establish accounting correctness: duplicate records and incorrectly extracted dates must also be reconciled.

## Privacy and security boundaries

- Use fictional data in any public demo. Do not commit accounting backups, employee data, receipts, PDFs, API keys, or screenshots containing private information.
- Full Backup files are **not encrypted**. They contain the underlying records and documents, not just a summary.
- Optional AI analysis sends document content to **Google Gemini**. Ordinary bookkeeping does not require an AI key. Do not send sensitive documents without assessing the service and your obligations.
- The Gemini key is held in application memory for the session; it is not included in the Full Backup. A browser-entered key is not a server-protected secret.
- Fonts are loaded from Google Fonts; SheetJS and pdfmake are loaded from a public CDN. Browser-local storage does not mean the page makes no external requests.
- There is no application-level login, role-based access control, cloud synchronization, or protected server-side accounting database.
- This project does not claim certified accounting, payroll, tax-filing, GoBD, or GDPR compliance. Reports require independent review before professional use.

## Verification status

Selected workflows were manually checked by the project owner on localhost during September 2026:

- Compatible backup preview and restore.
- Receipt gallery display and opening receipt images.
- PDF availability after restore.
- Full Backup download after restore.
- Accessing a historical year and correcting a transaction date.
- Reconciliation of dashboard counts and totals after reviewing migration duplicates and the incorrect date.

An exported backup was separately checked for embedded receipt content and PDF size/hash consistency. The submitted HTML also passed an inline JavaScript syntax check. These checks are **not** a complete browser regression or security audit.

### Known limitations and next steps

- Abrupt browser termination during restore has not been verified in a real-browser recovery test.
- Multi-tab concurrency and stale-state protection have not passed comprehensive browser testing. Use one active editing tab.
- Storage quota failures, eviction, and cross-browser behavior still need broader testing.
- AI extraction is fallible; manual confirmation remains necessary.
- Automated browser regression coverage and dependency/security review remain future work.

## Technology and design

**HTML · CSS · Vanilla JavaScript · localStorage · IndexedDB · SheetJS · pdfmake · optional Google Gemini API**

The single-file structure makes the application easy to inspect and run. It also creates a maintenance trade-off as the project grows. Future modularization should preserve the backup contract and add repeatable tests around storage, recovery, and rendering.

BuchWerk was developed iteratively with AI-assisted coding and hands-on workflow testing. Its engineering focus includes translating administrative needs into an interface, separating document binaries from metadata, validating portable backups, and handling persistence failures explicitly.

## Repository hygiene

Publish application source and documentation only. Keep operational data and backup files outside the project directory. Review the staged file list before every commit; avoid staging the entire working directory indiscriminately. Public source hosting does not itself provide a secure shared accounting service.
