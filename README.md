# The Miles Agency Reporting

Static reporting hub for The Miles Agency (Allstate, Lexington SC), deployed on Vercel. The root URL (`/`) is the report hub, with one card per report. Each report has an "← All Reports" link at the top of its sidebar (hidden when printed).

## Repository layout

```
public/                 # everything in here is deployed and publicly reachable
  index.html            # report hub (served at /)
  reports/
    sc-auto-campaign-configuration-2026-09-30.html   # /reports/sc-auto-campaign-configuration-2026-09-30
    sc-home-campaign-configuration-2026-09-30.html   # /reports/sc-home-campaign-configuration-2026-09-30
    geographic-targeting-2026-10-12.html             # /reports/geographic-targeting-2026-10-12 (map: 54 original + 96 added ZIPs)
  assets/
    sc-map-midlands.svg # SC county map with the 54 targeted ZIP codes (shared by both reports)
  robots.txt            # asks crawlers not to index the reports
vercel.json             # Vercel output directory, clean URLs, redirects, security headers
```

Files outside `public/` are not served; `outputDirectory` is pinned to `public`.

## Vercel project settings

Zero-build static site. When importing the repo into Vercel:

| Setting | Value |
| --- | --- |
| Framework Preset | **Other** |
| Build Command | *(leave empty)* |
| Output Directory | `public` (already set by `vercel.json`) |
| Install Command | *(leave empty)* |
| Root Directory | `./` |

No environment variables are required.

## What `vercel.json` does

- `outputDirectory: "public"` serves only the `public/` folder.
- `cleanUrls: true` serves `public/foo.html` at `/foo`.
- Short links: `/auto-configuration`, `/home-configuration` and `/geographic-targeting` redirect to their reports.
- `Cache-Control: max-age=0, must-revalidate` so updated reports show immediately.
- `X-Robots-Tag: noindex, nofollow` alongside `robots.txt`.
- `Content-Security-Policy` allows only inline style/script, `data:` images, same-origin images (the map) and Google Fonts.

## Design

Every page uses the GOAL reporting style: Sora headings and figures, Outfit body text, GOAL blue (`#057BE5`) as the one accent, and a light theme that switches to dark with the viewer's system setting. The round button at the top right toggles the theme and the choice is shared across the site (`localStorage` key `goal-theme`). Printing always uses the light theme. The SC map is inlined in the two configuration reports so it follows the theme; `assets/sc-map-midlands.svg` is the original source.

## Adding future reports

1. Save the report in `public/reports/` with a dated name, e.g. `campaign-performance-2026-11-15.html`.
2. Copy the head, sidebar and theme toggle of an existing report so it keeps the GOAL reporting style; the sidebar starts with `<a class="back" href="../index.html">&larr; All Reports</a>`.
3. Add a card for it in `public/index.html`.

## Access note

A Vercel deployment is reachable by anyone with the URL. To restrict it, enable Deployment Protection in Project Settings.

## Local preview

```bash
python3 -m http.server -d public 3000
```
