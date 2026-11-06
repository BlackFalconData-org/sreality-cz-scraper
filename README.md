# Sreality.cz Scraper [Just 💰$0.81]

Extract structured data from [sreality.cz](https://sreality.cz) — sreality.cz property listings for sale, rent and auction across all 14 Czech regions. Each row carries the price in CZK, floor area, GPS location and listing agency. Large searches are split automatically so a whole-country export is not cut off at 10,000 listings.

**[Sreality.cz Scraper [Just 💰$0.81] on Apify →](https://apify.com/blackfalcondata/sreality-cz-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N listings (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from sreality.cz on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from sreality.cz.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "category_main_cb=1&category_type_cb=1",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | What to search for. Free-text keywords such as `garáž` or `byt 2+kk` (words for sale/rent, flat/house/land, layouts and city names are understood; other words are matched in the listing text), a full sreality.cz search URL or its /hledani path with filters, or the filter part of a search such as `category_main_cb=1&category_type_cb=1`. Leave empty to scrape every listing on the portal. Use a JSON array for several searches in one run. |
| `location` | string | — | Optional. A Czech region, major city or district to restrict the search to, e.g. `Praha`, `Praha 5`, `Brno`, `Plzeň`, `Ostrava`, `Jihomoravský kraj` (accents optional), or the portal's region number 1–14. Leave empty for the whole country. A place that is not recognised stops the run with an input error. Use a JSON array for several regions in one run. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste sreality.cz search pages such as `https://www.sreality.cz/hledani/prodej/byty/praha` (transaction, category and region; also `zahranici` for listings abroad) or sreality.cz API search URLs. City pages (e.g. `/hledani/prodej/byty/brno`) and pages with price, area, floor, layout, ownership and text filters are supported; a page with any other filter stops the run with an input error that names the filter, instead of returning more listings than the page shows. Each URL becomes its own search task; results are deduped by listing ID. When provided, startUrls REPLACE the query/location fields; if none is supported the run stops with an input error. |
| `includeDetails` | boolean | `true` | Fetch full listing details. |
| `enrichEmails` | boolean | `false` | Add an `emails` list per listing: the email of the broker sreality.cz names on the listing, followed by any email written in the description. Needs Include Full Details. Off by default. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |

---

## Output fields

Every listing returns the same 67-field schema. Missing values are `null` — never omitted.

- `estate_id`
- `title`
- `offer_type`
- `offer_type_id`
- `category_main`
- `category_sub`
- `rooms`
- `source_url`
- `portalUrl`
- `locality_text`
- `location_city`
- `location_district`
- `location_region`
- `location_country`
- `location_latitude`
- `location_longitude`
- `street`
- `house_number`
- `price_czk`
- `price_czk_m2`
- `price_summary`
- `price_currency`
- `price_unit`
- `agency_id`
- `agency_slug`
- `agency_name`
- `has_video`
- `has_matterport`
- `primary_image_url`
- `image_urls`
- `images_count`
- `poi_atm_distance_m`
- `poi_bus_public_transport_distance_m`
- `poi_kindergarten_distance_m`
- `poi_medic_distance_m`
- `poi_metro_distance_m`
- `poi_playground_distance_m`
- `poi_post_office_distance_m`
- `poi_restaurant_distance_m`
- `poi_school_distance_m`
- `poi_shop_distance_m`
- `poi_small_shop_distance_m`
- `poi_train_distance_m`
- `poi_vet_distance_m`
- `description`
- `usable_area_m2`
- `land_area_m2`
- `building_type`
- `building_condition`
- `ownership`
- `energy_rating`
- `floor`
- `total_floors`
- `year_built`
- `listed_at`
- `updated_at`
- `extracted_emails`
- `extracted_phones`
- `scraped_at`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "estate_id": 2702176332,
  "title": "Prodej bytu 3+kk 78 m²",
  "offer_type": "Prodej",
  "offer_type_id": 1,
  "category_main": "Byty",
  "category_sub": "3+kk",
  "rooms": "3+kk",
  "source_url": "https://www.sreality.cz/detail/prodej/byt/3+kk/praha-karlin-sokolovska/2702176332",
  "portalUrl": "https://www.sreality.cz/detail/prodej/byt/3+kk/praha-karlin-sokolovska/2702176332",
  "locality_text": "Praha, Praha 8, Hlavní město Praha, Česká republika",
  "location_city": "Praha",
  "location_district": "Praha 8"
}
```

*Truncated — full records contain 67 fields. See Output fields for the complete schema.*

**[Try Sreality.cz Scraper [Just 💰$0.81] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/sreality-cz-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.00081 |

See the [actor on Apify](https://apify.com/blackfalcondata/sreality-cz-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape sreality.cz?**
Use this actor on Apify to extract structured data from sreality.cz. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get sreality.cz data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape sreality.cz?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check sreality.cz's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Sreality.cz Scraper [Just 💰$0.81]](https://apify.com/blackfalcondata/sreality-cz-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
