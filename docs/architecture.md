# Architecture

## Overview

The QEF website follows the same publishing pattern as the school photo gallery, but with a QEF-specific data contract.

```text
Static frontend on GitHub Pages
  ↓ JSONP request
Apps Script read-only API
  ↓
Google Sheet QEF tabs
  ↓
Google Drive image IDs or folder IDs
```

The frontend can also run without an API URL by falling back to sample content in `config.js`.

## Frontend

- `index.html` holds semantic page regions and static asset references.
- `config.js` holds public configuration and fallback content.
- `assets/styles.css` defines the visual system and responsive layout.
- `assets/app.js` loads API data, normalizes records, renders navigation, pages, metrics, course-content cards, album-style detail pages, and photo mosaics.

## Backend

`apps-script/Code.gs` exposes:

- `?action=site`: full QEF website payload.
- `?action=health`: API status check.

The API supports JSON and JSONP. GitHub Pages should use JSONP for the same deployment style as `shine-photo-gallery`.

`QEF_Pages` is the single public content table. It contains the seven main site
sections and all `課程內容` rows. `QEF_Photos` is no longer read by the API; cover
images come from `QEF_Pages.封面圖片ID`, and galleries come from
`QEF_Pages.資料夾ID`.

The frontend renders sample content immediately while the live Apps Script payload loads. This keeps the public page usable during Apps Script cold starts or long Drive scans; the live payload replaces the sample content when it arrives.

`apps-script/Code.gs` uses Apps Script `CacheService` for:

- full `site` payload cache, about 10 minutes;
- per-page folder photo metadata cache, up to 6 hours.

Use `clearQefCache()` or `warmQefSiteCache()` in the Apps Script editor after redeploying the backend or changing Drive folder contents.

## Security

- The API is read-only.
- No frontend secrets are used.
- Google Drive image access depends on Drive sharing and the Apps Script executing account.
- Do not place private student data in public-facing Sheet fields.
