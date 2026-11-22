# Tesla Fire Systems Inc — Canada product feeds

**Entity:** Tesla Fire Systems Inc (Canada) · **Store:** https://teslafiresystems.com · **Feeds served at:** https://data.teslafiresystems.com

This repo holds the Canadian feeds for Google Merchant Center and ChatGPT (OpenAI) Ads. The US feeds for Tesla Fire Systems LLC (teslafire.com) live in a separate repo: [SKGroupstor/tesla-fire-systems-llc-usa-feeds](https://github.com/SKGroupstor/tesla-fire-systems-llc-usa-feeds).

## Feeds

| Feed | Platform | File | Public URL |
|---|---|---|---|
| Local inventory feed | Google Merchant Center | `GMC-local-inventory-feed-inc.csv` | https://data.teslafiresystems.com/GMC-local-inventory-feed-inc.csv |
| Product feed (upload file) | ChatGPT (OpenAI) Ads | `open ai/teslafiresystems_openai_feed.jsonl` | https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.jsonl |
| Product feed (review copy) | ChatGPT (OpenAI) Ads | `open ai/teslafiresystems_openai_feed.csv` | https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.csv |

Raw fallback URLs (if GitHub Pages is ever down): replace `https://data.teslafiresystems.com/` with `https://raw.githubusercontent.com/SKGroupstor/tesla-fire-systems-inc-canada-feeds/main/`.

### Google Merchant Center
- The Inc **primary** product feed comes from the WooCommerce "Google Listings & Ads" plugin (ids like `gla_17262`). This repo only supplies the **local inventory** supplement.
- Register the local inventory URL above as the scheduled-fetch URL in Merchant Center.

### ChatGPT (OpenAI) Ads
- Canada-only ad account; 313 products, prices in CAD, `target_countries: ["CA"]`.
- OpenAI Ads Manager takes the feed by **SFTP upload** (Tools → Feeds → Create Feed). The URLs are for download and reference.
- Feed settings and notes: [`open ai/README.md`](open%20ai/README.md).

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
├── backups/                               dated snapshots and historical Merchant Center exports
└── .github/workflows/backup-on-update.yml backs up the pre-push version of any modified file into backups/
```

Moved here from `SKGroupstor/tesla-merchant-centre` on 2026-11-22.
