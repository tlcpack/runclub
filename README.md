# Neighborhood Run Club

A zero-cost, zero-backend leaderboard. Times live in a Google Sheet; the site
reads the published CSV directly in the browser and renders a per-distance
leaderboard. No database, no server.

- **Hosting:** GitHub Pages (free static hosting + SSL)
- **Data / admin UI:** a published Google Sheet (mobile-friendly entry at the track)
- **Frontend:** single `index.html` using [PapaParse](https://www.papaparse.com/) (CSV) + [Grid.js](https://gridjs.io/) (table)

## Setup

### 1. The Google Sheet

The workbook has one tab per distance (`100m`, `400m`, `800m`, `1 Mile`),
each wide-format with one row per runner:

| Bib No. | Runner | Week 1 (07/08/26) | Week 2 | … | Week 8 | Personal Best | Improvement |
|---------|--------|-------------------|--------|---|--------|---------------|-------------|

- Week columns are matched by header (`Week <n>`, with an optional `(date)`
  that shows up in the site's week filter).
- `Personal Best` / `Improvement` are ignored — the site computes bests itself.
- Times are parsed by shape: `1:57:19` → 1:57.19 (minutes:seconds:hundredths),
  `5:42.0` → minutes:seconds, plain numbers are seconds on sprints (`13.91`)
  and minutes.seconds shorthand on 800m / 1 Mile (`8.19` → 8:19, `4` → 4:00).
- **Pace is computed for you** — don't add a Pace column.
- The **Master Roster** tab (parent contact info) is never fetched by the site.

### 2. Share it publicly

`Share` → **General access** → **Anyone with the link** → **Viewer**.
The site reads the sheet's gviz CSV endpoint, which requires this.

### 3. Wire it up

In `index.html`, set `SHEET_ID` (from the edit URL,
`.../spreadsheets/d/<SHEET_ID>/edit`). Tabs are fetched by name via:

```js
'https://docs.google.com/spreadsheets/d/' + SHEET_ID + '/gviz/tq?tqx=out:csv&sheet=' + encodeURIComponent(sheet);
```

so tab names in the sheet must match the keys of the `DISTANCES` map.

### 4. Enable GitHub Pages

Repo **Settings > Pages** → Source: deploy from the `main` branch, `/ (root)`.
The site goes live at `https://tlcpack.github.io/runclub/`.

## Notes

- Published-CSV changes propagate in a few minutes (Google caches it).
- Times sort fastest→slowest correctly because they're parsed to seconds before sorting.
- To add a distance, edit the `DISTANCES` map in `index.html`.
