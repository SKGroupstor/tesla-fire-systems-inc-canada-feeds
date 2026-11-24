# Tesla Fire Systems Inc — Canada product feeds

**Entity:** Tesla Fire Systems Inc (Canada) · **Store:** https://teslafiresystems.com · **Feeds served at:** https://data.teslafiresystems.com

This repo holds the Canadian feeds for Google Merchant Center and ChatGPT (OpenAI) Ads. The US feeds for Tesla Fire Systems LLC (teslafire.com) live in a separate repo: [SKGroupstor/tesla-fire-systems-llc-usa-feeds](https://github.com/SKGroupstor/tesla-fire-systems-llc-usa-feeds).

## Feeds

| Feed | Platform | File | Public URL |
|---|---|---|---|
| Local inventory feed | Google Merchant Center | `GMC-local-inventory-feed-inc.csv` | https://data.teslafiresystems.com/GMC-local-inventory-feed-inc.csv |
| Product feed — **hosted URL for Ads Manager** | ChatGPT (OpenAI) Ads | `open ai/teslafiresystems_openai_feed.tsv` | https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.tsv |
| Product feed — manual upload (TXT / CSV) | ChatGPT (OpenAI) Ads | `open ai/teslafiresystems_openai_feed.txt`, `.csv` | https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.txt |
| Product feed — SFTP / API | ChatGPT (OpenAI) Ads | `open ai/teslafiresystems_openai_feed.jsonl` | https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.jsonl |

Raw fallback URLs (if GitHub Pages is ever down): replace `https://data.teslafiresystems.com/` with `https://raw.githubusercontent.com/SKGroupstor/tesla-fire-systems-inc-canada-feeds/main/`.

### Google Merchant Center
- The Inc **primary** product feed comes from the WooCommerce "Google Listings & Ads" plugin (ids like `gla_17262`). This repo only supplies the **local inventory** supplement.
- Register the local inventory URL above as the scheduled-fetch URL in Merchant Center.

### ChatGPT (OpenAI) Ads
- Canada-only ad account; 313 products, prices in CAD, `target_countries: ["CA"]`.
- In Ads Manager (Tools → Feeds → ⋮) use **Hosted URL** with the `.tsv` link so it refreshes automatically, or **Upload CSV or TXT** with the `.txt`/`.csv` file. The `.jsonl` is for SFTP/API only.
- Feed settings and notes: [`open ai/README.md`](open%20ai/README.md).

## Automatic updates

All feeds in this repo rebuild themselves from the live store (WooCommerce Store API). A run only commits when something changed: price, sale price, stock, title, description, images, new or removed products.

| When | What happens |
|---|---|
| **Before the Merchant Center deadline (3:00 AM EST)** | 1:17 AM and 2:17 AM Toronto: rebuild and republish if anything changed. The second run is a safety net. |
| **Daytime checks** | 10:17 AM and 4:17 PM Toronto. |
| **Weekly forced refresh** | Mondays 1:41 AM Toronto: republishes and stamps `feed-status.json` even if nothing changed. |
| **On demand** | Actions tab → *Update feeds* → *Run workflow* (tick *force* for a weekly-style refresh). |

- **Primary trigger:** the Cloudflare Worker `skgroups-monitor-scheduler` (SK GROUPS account) starts the *Update feeds* workflow on time (Toronto time, EST/EDT handled automatically).
- **Backup trigger:** the GitHub cron in `.github/workflows/update-feeds.yml` runs ~30 minutes after each Cloudflare time. A second run finds no changes and does nothing.
- Code: `scripts/build-feeds.mjs` (Node 20, no dependencies); settings: `feeds.config.json`.
- `feed-status.json` shows when the feeds last changed, the last weekly refresh, and product counts.
- Google feed rows keep their curated settings (ids, pickup settings, GTIN/MPN); prices, stock and text come from the store. Deleted products are removed; products published after 2026-11-22 are added automatically.
- If the store returns fewer than 50 products (outage, block), the run stops without touching the feeds.
- Every automated change is a commit, so any earlier version can be restored from the git history.

## Hosting
- GitHub Pages from the `main` branch root, custom domain `data.teslafiresystems.com` (set by the `CNAME` file).
- DNS (Cloudflare): `CNAME data → skgroupstor.github.io`, proxy **off** (DNS only), so GitHub can issue the HTTPS certificate.
- Anything pushed to `main` is live within moments.

## Layout
```
├── CNAME                                  data.teslafiresystems.com
├── GMC-local-inventory-feed-inc.csv       Google local inventory feed (252 rows)
├── open ai/                               ChatGPT (OpenAI) Ads feed
│   ├── teslafiresystems_openai_feed.jsonl
│   ├── teslafiresystems_openai_feed.csv
│   └── README.md
├── scripts/build-feeds.mjs                rebuilds all feeds from the live store
├── feeds.config.json                      store, country, seller and feed file settings
├── feed-status.json                       last change / last weekly refresh / product counts
├── backups/                               dated snapshots and historical Merchant Center exports
└── .github/workflows/
    ├── update-feeds.yml                   daily crawl + weekly refresh
    ├── publish-gh-pages.yml               mirrors main to gh-pages (the Pages branch)
    └── backup-on-update.yml               backs up the pre-push version of hand-edited files into backups/
```

Moved here from `SKGroupstor/tesla-merchant-centre` on 2026-11-22.
