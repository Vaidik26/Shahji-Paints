# Shaji Paints – Operations Control Tower (Vercel, Excel data file)

A static site. There is no server code and no database: the Excel workbook
`public/data/control-tower-data.xlsx` is the data store.

## Project layout

```
public/
  index.html                        the dashboard (single file)
  data/control-tower-data.xlsx      the data workbook the dashboard loads at start
vercel.json                         serves the public/ folder; no caching of the data file
```

## Deploy

1. Put this folder in a **private** Git repository (GitHub, GitLab or Bitbucket).
2. In Vercel: Add New > Project > import the repository.
   Framework preset: **Other**. No build command. Output directory: `public`
   (already set in vercel.json).
3. Deploy. Open the site: it loads the data workbook and shows the latest review.

Alternative without Git: install the Vercel CLI and run `vercel --prod` in this folder.

## Protect the data

The data workbook is served with the site, so anyone who can open the site can
download it. Keep the repository private and turn on Deployment Protection for the
project in Vercel (Settings > Deployment Protection), or do not publish the data
file at all: delete `public/data/control-tower-data.xlsx` and have each user load
their copy with **Open data file**.

## How data flows

- On opening, the dashboard loads `data/control-tower-data.xlsx` and keeps a
  working copy in the browser (IndexedDB), so nothing is lost on reload.
- Uploads, Monthly inputs, Settings, employee changes and month locks all work as
  before. They change the working copy in that browser, and the header shows
  **Unsaved changes**.
- **Save data to Excel** downloads a new `control-tower-data.xlsx` with everything.
  To share it, replace `public/data/control-tower-data.xlsx` in the repository
  with that file and push (Vercel redeploys). Everyone who opens the site then
  gets the new data.
- **Open data file** loads a data workbook from your computer instead.
- If a newer data file is published while a browser still has unsaved changes,
  the dashboard asks whether to keep the unsaved changes or load the new file.
- One person should own the data file at a time: two people saving separate
  copies cannot be merged automatically.

## Data workbook

| Sheet | Purpose |
|---|---|
| Read Me | What the file is and how to publish it |
| Months | Each month: locked/open, datasets, source files, STO raised, prepared by |
| Monthly inputs | Stock accuracy, 5S, CFGS inputs, attendance, leave, STO, per month |
| Employees | Employee master: branch, roles, status |
| Settings | Scoring rules and settings |
| Data | **The sheet the dashboard reads** (JSON split into 30,000-character parts). Do not edit. |

The readable sheets are rewritten on every save; edits made in them are not read back.
Change data through the dashboard.

## Requirements

A modern browser (Chrome, Edge, Firefox, Safari) with internet access: the page loads
SheetJS, html2canvas and jsPDF from cdnjs.cloudflare.com.
