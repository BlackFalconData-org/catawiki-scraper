# Catawiki Scraper [Just 💰$0.75]

Extract structured data from [catawiki.com](https://catawiki.com) — Catawiki auction lots by keyword, category or lot URL. Each row carries the live bid, the full bid history, the expert estimate and the seller profile with feedback counts. Track open auctions run to run and receive only new or changed lots.

**[Catawiki Scraper [Just 💰$0.75] on Apify →](https://apify.com/blackfalcondata/catawiki-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from catawiki.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from catawiki.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "lego",
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
| `query` | string | — | Lot search keywords (e.g. "lego", "rolex", "banksy"). Required if no startUrls provided. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste Catawiki pages: a search URL (catawiki.com/en/s?q=...), a category page (/c/697-omega-watches) or a single lot page (/l/107908720-...). Each URL becomes its own task; results are merged and deduped by lot ID. When provided, start URLs REPLACE the query field (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch full listing details. |
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

Every product returns the same 99-field schema. Missing values are `null` — never omitted.

- `lotId`
- `url`
- `title`
- `subtitle`
- `auctionId`
- `currentBid`
- `currentBidCurrency`
- `currentBidEur`
- `currentBidUsd`
- `currentBidGbp`
- `bidCount`
- `lastBidAt`
- `bidHistory`
- `biddingStartTime`
- `biddingEndTime`
- `isClosed`
- `isOpen`
- `isSold`
- `reservePriceSet`
- `reservePriceMet`
- `buyNowAvailable`
- `buyNowPriceEur`
- `favoriteCount`
- `highInterest`
- `hasFreeShipping`
- `imageUrl`
- `thumbnailUrl`
- `images`
- `imageCount`
- `description`
- `descriptionTranslated`
- `summary`
- `estimateMin`
- `estimateMax`
- `specifications`
- `categoryId`
- `categoryUrl`
- `categoryLevel`
- `expertName`
- `expertId`
- `sellerMemberSince`
- `sellerLotsInAuction`
- `sellerPositiveCount`
- `sellerNeutralCount`
- `sellerNegativeCount`
- `sellerBadges`
- `ageCheckRequired`
- `hasExplicitContent`
- `isContentExplicit`
- `sellerId`
- `sellerName`
- `sellerUrl`
- `sellerCountry`
- `sellerCountryCode`
- `sellerIsPro`
- `sellerIsTop`
- `sellerScore`
- `sellerFeedbackCount`
- `sellerObjectsSold`
- `realtimeChannel`
- `scrapedAt`
- `listingId`
- `id`
- `portalUrl`
- `slug`
- `locale`
- `thumbnailImageUrl`
- `sellerShopName`
- `sourceType`
- `scrapedDate`
- `descriptionText`
- `descriptionMarkdown`
- `minimumNextBid`
- `startingBid`
- `estimateCurrency`
- `biddingEndDate`
- `categoryL0Id`
- `categoryL0Name`
- `categoryL1Id`
- `categoryL1Name`
- `categoryL2Id`
- `categoryL2Name`
- `auctionTitle`
- `auctionUrl`
- `auctionStatus`
- `auctionStartAt`
- `auctionCloseAt`
- `auctionLotCount`
- `auctionIsThemed`
- `expertUrl`
- `specs`
- `isRepost`
- `changeStatus`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "lotId": 107870621,
  "url": "https://www.catawiki.com/en/l/107870621-omega-geneve-no-reserve-price-135-012-men-1960-1969",
  "title": "Omega - Genève - No reserve price - 135.012 - Men - 1960-1969",
  "subtitle": "Manual winding - Stainless steel",
  "auctionId": 1296434,
  "currentBid": 281,
  "currentBidCurrency": "EUR",
  "currentBidEur": 281,
  "currentBidUsd": 316,
  "currentBidGbp": 236,
  "bidCount": 40,
  "lastBidAt": "2026-10-28T15:34:09Z"
}
```

*Truncated — full records contain 99 fields. See Output fields for the complete schema.*

**[Try Catawiki Scraper [Just 💰$0.75] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/catawiki-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.005 |
| Result | $0.00075 |

See the [actor on Apify](https://apify.com/blackfalcondata/catawiki-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape catawiki.com?**
Use this actor on Apify to extract structured data from catawiki.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get catawiki.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape catawiki.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check catawiki.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Catawiki Scraper [Just 💰$0.75]](https://apify.com/blackfalcondata/catawiki-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
