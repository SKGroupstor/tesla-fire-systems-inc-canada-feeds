# ChatGPT (OpenAI) Ads product feed — teslafiresystems.com

Product feed for OpenAI Ads Manager (Canada-only ad account), built from the live WooCommerce catalog at https://teslafiresystems.com.

| File | Purpose |
|---|---|
| `teslafiresystems_openai_feed.jsonl` | Upload file (OpenAI product feed spec, one product per line) |
| `teslafiresystems_openai_feed.csv` | Same feed as a spreadsheet for review |

## Links
- **Hosted URL for Ads Manager (recommended):** https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.tsv
- Manual upload (Upload CSV or TXT): `teslafiresystems_openai_feed.txt` (tab-delimited) or `teslafiresystems_openai_feed.csv`
- TXT: https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.txt
- CSV: https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.csv
- JSONL (SFTP / API): https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.jsonl

## Feed settings
- Seller: Tesla Fire Systems Inc
- Market: `target_countries: ["CA"]`, `store_country: "CA"`, prices in CAD
- `is_eligible_search: true`, `is_ads_eligible: true` on every row
- `item_id` = WooCommerce product ID (stable)
- Products with no brand on the site use `Tesla Fire Systems` as the brand
- Excluded: $0 free promo items (Free Maintenance Tags, Free safety tie)

## Upload
Ads Manager → Tools → Feeds → open the feed's ⋮ menu:
- **Hosted URL** (best — Ads Manager re-fetches it, and this repo rebuilds it daily): paste `https://data.teslafiresystems.com/open%20ai/teslafiresystems_openai_feed.tsv`
- **Upload CSV or TXT**: upload `teslafiresystems_openai_feed.txt` (or `.csv`)

Ads Manager does not accept the `.jsonl` file in its upload box; JSONL is for SFTP/API delivery only.

Spec: https://developers.openai.com/commerce/specs/file-upload/products
