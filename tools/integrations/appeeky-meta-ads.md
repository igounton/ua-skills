# Appeeky Meta Ads

Meta Ad Library competitive intelligence + Marketing API campaign management via OAuth.

MCP tool arguments use **snake_case**. REST query/body fields use **camelCase**.

## Ad Library (read-only, API key)

No Meta token required — Appeeky uses the public Ad Library scraper.

| Method | Path | MCP Tool |
|--------|------|----------|
| `GET /v1/ads/meta/search` | Search Ad Library | `meta_ads_search` |
| `GET /v1/ads/meta/pages/:pageId/ads` | Page ads | `meta_ads_page_ads` |
| `GET /v1/apps/:id/ad-intelligence` | App → brand ads | `meta_app_ad_intelligence` |

## Marketing API (OAuth connect)

Base: `/v1/connect/meta-ads/`

MCP create/list tools POST/GET the un-nested paths (`/campaigns`, `/adsets`, `/ads`, `/creatives`) and pass `ad_account_id`. Nested `/ad-accounts/:adAccountId/...` aliases also exist.

| Area | REST | MCP |
|------|------|-----|
| OAuth | `oauth/start`, `credentials/status` | `meta_ads_oauth_start`, `meta_ads_credentials_status` |
| Accounts | `ad-accounts`, `pages` | `meta_ads_list_ad_accounts`, `meta_ads_list_pages` |
| Apps / identities | `advertisable-applications`, `publisher-identities` | `meta_ads_list_advertisable_applications`, `meta_ads_list_publisher_identities` |
| One-shot app install | `POST app-install-drafts` | `meta_ads_create_app_install_draft` |
| Campaigns | `campaigns` GET/POST/PATCH | `meta_ads_list_campaigns`, `meta_ads_create_campaign`, `meta_ads_update_campaign` |
| Ad sets | `adsets` GET/POST/PATCH | `meta_ads_create_adset`, `meta_ads_list_adsets`, `meta_ads_update_adset` |
| Ads | `ads` GET/POST/PATCH | `meta_ads_create_ad`, `meta_ads_list_ads`, `meta_ads_update_ad` |
| Creatives | `creatives` GET/POST | `meta_ads_create_creative`, `meta_ads_list_creatives`, `meta_ads_get_creative` |
| Images | `POST adimages/from-url` | `meta_ads_upload_ad_image_from_url` (`image_url`) |
| Insights | `insights` GET | `meta_ads_insights` |

Writes cost **2 API credits**.

## Create App Install Draft

Creates a PAUSED campaign + ad set + creative + ad. Does not require a Facebook Page if an Instagram professional account is available.

API default `optimization_goal` is `APP_INSTALLS`. **Override for subscription/IAP apps is not enough** — this tool does not set `custom_event_type` on `promoted_object`. For Purchase/Subscribe, use `meta_ads_create_campaign` + `meta_ads_create_adset` instead.

Games / ad-supported only:

```
meta_ads_create_app_install_draft
  application_id: "<from meta_ads_list_advertisable_applications>"
  object_store_url: "https://apps.apple.com/us/app/id123"
  daily_budget_minor: 5000
  message: "Primary text"
  headline: "Headline"
  image_hash: "<from upload>"
  page_id: "<optional facebook page>"
  destination_type: "APP"
  optimization_goal: "APP_INSTALLS"
```

## Create Ad Creative (MCP)

`name` is required. For app install ads set `format: "app_install"` (default is `"link"`).

```
meta_ads_create_creative
  format: "app_install"
  name: "FocusApp - VarA"
  page_id: "123456789"
  link_url: "https://apps.apple.com/app/id123"
  message: "Primary text here"
  headline: "Headline here"
  image_hash: "abc123"
  call_to_action_type: "INSTALL_MOBILE_APP"
```

REST body uses camelCase (`pageId`, `linkUrl`, `imageHash`, `callToActionType`).

## Create Ad Set (app install)

MCP defaults `destination_type` to `WEBSITE`. App campaigns must override:

```
meta_ads_create_adset
  campaign_id: "<id>"
  name: "US - iOS - Purchase"
  daily_budget_minor: 5000
  billing_event: "IMPRESSIONS"
  optimization_goal: "OFFSITE_CONVERSIONS"
  destination_type: "APP"
  promoted_object: {
    "application_id": "<meta-app-id>",
    "object_store_url": "https://apps.apple.com/us/app/id123",
    "custom_event_type": "PURCHASE"
  }
  targeting: { "geo_locations": { "countries": ["US"] } }
  status: "PAUSED"
```

## Insights Params (MCP)

- `target_type`: `ad_account`, `campaign`, `adset`, `ad`
- `target_id`: entity ID
- `date_preset`: `last_7d`, `last_14d`, `last_30d`
- `fields`: `spend`, `impressions`, `clicks`, `actions`, `cpc`, `cpm`

REST query uses `targetType`, `targetId`, `datePreset`.
