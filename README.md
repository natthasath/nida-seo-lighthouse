# 🎉 NIDA SEO Lighthouse

NIDA SEO Lighthouse is a lightweight project that automates SEO audits using Google Lighthouse, helping teams analyze performance, accessibility, best practices, and SEO metrics to improve web quality and search visibility.

![workflow](https://img.shields.io/github/actions/workflow/status/natthasath/nida-seo-lighthouse/lighthouse.yaml?label=lighthouse)
![license](https://img.shields.io/github/license/natthasath/nida-seo-lighthouse)
![last commit](https://img.shields.io/github/last-commit/natthasath/nida-seo-lighthouse)

### ✨ Features

- Automated weekly Lighthouse audits (Mobile + Desktop) for `nida.ac.th`
- Scores across Performance, Accessibility, Best Practices, SEO, and PWA
- Auto-generates a static dashboard (`index.html`) with filterable report history
- Report metadata indexed in `reports.json`
- Full HTML reports archived under `report/mobile/` and `report/desktop/`

### 🧊 Tech Stack

| Component | Tool |
| --- | --- |
| Audit engine | [Lighthouse CLI](https://github.com/GoogleChrome/lighthouse) |
| CI orchestration | [@lhci/cli](https://github.com/GoogleChrome/lighthouse-ci), GitHub Actions |
| Report dashboard | Static HTML (`index.html`), no framework |

### 🚀 Setup

This project runs entirely through GitHub Actions — no local installation required to use it.

To trigger a run manually: go to **Actions → Weekly Lighthouse Report → Run workflow**.

To reproduce the same audit locally:

```shell
npm install -g lighthouse
npm install -g @lhci/cli@0.13.x

# Mobile
lighthouse https://nida.ac.th \
  --output=html \
  --output-path="report/mobile/$(date +%F)_lighthouse-report.html" \
  --chrome-flags="--headless --no-sandbox --disable-gpu" \
  --throttling-method=simulate \
  --only-categories=performance,accessibility,best-practices,seo,pwa

# Desktop
lighthouse https://nida.ac.th \
  --output=html \
  --output-path="report/desktop/$(date +%F)_lighthouse-report.html" \
  --preset=desktop \
  --chrome-flags="--headless --no-sandbox --disable-gpu" \
  --only-categories=performance,accessibility,best-practices,seo,pwa
```

### ⚙️ Configuration

| Setting | Value |
| --- | --- |
| Target URL | `https://nida.ac.th` |
| Categories | performance, accessibility, best-practices, seo, pwa |
| Mobile throttling | simulated (default Lighthouse mobile emulation) |
| Desktop preset | `--preset=desktop` |

To audit a different site, update the target URL in [`.github/workflows/lighthouse.yaml`](.github/workflows/lighthouse.yaml).

### 🏆 Usage

Each scheduled run commits fresh reports and regenerates the dashboard automatically:

- `report/mobile/{date}_lighthouse-report.html` — mobile report for that run
- `report/desktop/{date}_lighthouse-report.html` — desktop report for that run
- `reports.json` — machine-readable index of all reports (filename, date, URL, size)
- `index.html` — static dashboard listing every report with All / Mobile / Desktop filters

Open `index.html` in a browser, or via GitHub Pages if enabled for this repo, to browse the report history.

### 📅 Schedule

Runs automatically every Monday at 02:00 UTC (09:00 ICT), or on demand via `workflow_dispatch`.

```yaml
on:
  schedule:
    - cron: '0 2 * * 1'
  workflow_dispatch:
```

### 📜 License

MIT © 2023 Natthasath Saksupanara — see [LICENSE](LICENSE)
