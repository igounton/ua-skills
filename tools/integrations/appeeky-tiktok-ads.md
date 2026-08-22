# Appeeky TikTok Ads

TikTok Marketing API via OAuth — campaigns, ad groups, ads, assets, performance.

Base: `/v1/connect/tiktok-ads/`

MCP tool arguments use **snake_case**. REST query/body fields use **camelCase**.

## OAuth & Accounts

| Endpoint | MCP Tool |
|----------|----------|
| `oauth/start` | `tiktok_ads_oauth_start` |
| `credentials/status` | `tiktok_ads_credentials_status` |
| `advertisers` | `tiktok_ads_list_advertisers` |
| `advertisers/:id/default` PATCH | `tiktok_ads_set_default_advertiser` |

## Campaign Management

| Endpoint | MCP Tool |
|----------|----------|
| `campaigns` GET/POST | `tiktok_ads_list_campaigns`, `tiktok_ads_create_campaign` |
| `campaigns/:id` PATCH | `tiktok_ads_update_campaign` (`campaign_id` + `payload`) |
| `campaigns/:id/status` POST | `tiktok_ads_update_campaign_status` (`campaign_id`, `operation_status`) |
| `adgroups` GET/POST | `tiktok_ads_list_adgroups`, `tiktok_ads_create_adgroup` |
| `adgroups/:id/status` POST | `tiktok_ads_update_adgroup_status` (`adgroup_id`, `operation_status`) |
| `ads` GET/POST | `tiktok_ads_list_ads`, `tiktok_ads_create_ad` |
| `ads/:id/status` POST | `tiktok_ads_update_ad_status` (`ad_id`, `operation_status`) |
| `aco-ads` POST/PATCH | `tiktok_ads_create_aco_ad`, `tiktok_ads_update_aco_ad` |

## Assets & Spark

| Endpoint | MCP Tool |
|----------|----------|
| `identities` GET | `tiktok_ads_list_identities` (`advertiser_id`) |
| `assets/images/from-url` POST | `tiktok_ads_upload_image_from_url` (`image_url`) |
| `assets/videos/from-url` POST | `tiktok_ads_upload_video_from_url` (`video_url`) |

Spark Ads: `tiktok_ads_create_ad` with a `creatives` array containing `identity_id` + `tiktok_item_id` (or spark authorization code).

## Reporting

| Endpoint | MCP / REST |
|----------|------------|
| `performance` GET | MCP `tiktok_ads_performance` |
| `report` GET | REST only — **no MCP tool**. Use `tiktok_ads_performance` from skills. |

### Performance Params (MCP)

- `advertiser_id` (optional; defaults to connected advertiser)
- `level`: `advertiser`, `campaign`, `adgroup`, `ad`
- `days` or `from`/`to`
- Returns: spend, impressions, clicks, installs, CPA, CPC, CTR

## Typical App Install Flow

### 1. Campaign

```
tiktok_ads_create_campaign
  advertiser_id: "<id>"
  campaign_name: "App - App Install - US Test"
  objective_type: "APP_PROMOTION"
  budget_mode: "BUDGET_MODE_DAY"
  budget: 50
```

### 2. Ad group

`schedule_start_time` is required. `placements` and `location_ids` are comma-separated strings. Extra TikTok fields go in `payload`.

```
tiktok_ads_create_adgroup
  advertiser_id: "<id>"
  campaign_id: "<id>"
  adgroup_name: "US - Purchase Opt - TikTok Only"
  budget: 50
  budget_mode: "BUDGET_MODE_DAY"
  billing_event: "OCPM"
  optimization_goal: "IN_APP_EVENT"
  schedule_start_time: "2026-08-22 12:00:00"
  schedule_type: "SCHEDULE_FROM_NOW"
  placements: "PLACEMENT_TIKTOK"
  location_ids: "6252001"
  payload:
    promotion_type: "APP_INSTALL"
    placement_type: "PLACEMENT_TYPE_NORMAL"
```

### 3. Ads

`tiktok_ads_create_ad` requires `adgroup_id` + `creatives` array (raw TikTok `/ad/create` objects). Do not pass `ad_name` / `identity_id` as top-level MCP args.

```
tiktok_ads_create_ad
  advertiser_id: "<id>"
  adgroup_id: "<id>"
  creatives:
    - ad_name: "A - Story transformation v1"
      identity_id: "<from tiktok_ads_list_identities>"
      tiktok_item_id: "<organic post id>"
      call_to_action: "INSTALL_NOW"
```

### 4. Audit

```
tiktok_ads_performance
  advertiser_id: "<id>"
  level: "ad"
  days: 3
```
