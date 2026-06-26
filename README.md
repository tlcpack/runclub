# Neighborhood Run Club

A zero-cost, zero-backend leaderboard. Times live in a Google Sheet; the site
reads the published CSV directly in the browser and renders a per-distance
leaderboard. No database, no server.

- **Hosting:** GitHub Pages (free static hosting + SSL)
- **Data / admin UI:** a published Google Sheet (mobile-friendly entry at the track)
- **Frontend:** single `index.html` using [PapaParse](https://www.papaparse.com/) (CSV) + [Grid.js](https://gridjs.io/) (table)

## Setup

### 1. Create the Google Sheet

Make a sheet with this header row (exact names matter):

| Name | Date | Distance | Time |
|------|------|----------|------|

- **Distance** must be one of: `100m`, `200m`, `400m`, `1 mi`
- **Time** is seconds (`13.4`) or minutes:seconds (`5:42.0`)
- **Pace is computed for you** — don't add a Pace column.

### 2. Share it publicly

`Share` → **General access** → **Anyone with the link** → **Viewer**.
The site reads the sheet's CSV export endpoint, which requires this.

### 3. Wire it up

In `index.html`, set `CSV_URL` to the sheet's CSV export URL:

```js
const CSV_URL = 'https://docs.google.com/spreadsheets/d/<SHEET_ID>/export?format=csv&gid=<TAB_GID>';
```

`<SHEET_ID>` and `<TAB_GID>` come from the normal edit URL
(`.../spreadsheets/d/<SHEET_ID>/edit?gid=<TAB_GID>`). Header whitespace is
tolerated, so stray trailing spaces in column names won't break parsing.

### 4. Enable GitHub Pages

Repo **Settings > Pages** → Source: deploy from the `main` branch, `/ (root)`.
The site goes live at `https://tlcpack.github.io/runclub/`.

## Notes

- Published-CSV changes propagate in a few minutes (Google caches it).
- Times sort fastest→slowest correctly because they're parsed to seconds before sorting.
- To add a distance, edit the `DISTANCES` map in `index.html`.
