# Appeeky Apple Search Ads

ASA campaign management, keyword reports, and RevenueCat profitability joins.

Base: `/v1/connect/apple-ads/`

MCP tool arguments use **snake_case** (`campaign_id`, `ad_group_id`, `app_id`). REST path/query fields are camelCase.

## Credentials

Store via `POST /credentials` or per-request headers: `X-ASA-Client-Id`, `X-ASA-Team-Id`, `X-ASA-Key-Id`, `X-ASA-Org-Id`, `X-ASA-Private-Key`.

MCP: `asa_credentials_status`

## Campaign Ops

| MCP Tool | Purpose | Key args |
|----------|---------|----------|
| `asa_list_campaigns` | List campaigns | |
| `asa_create_campaign` | New SEARCH campaign | `adam_id`, `name`, `countries`, `daily_budget_amount`, `daily_budget_currency` |
| `asa_list_adgroups` | List ad groups | `campaign_id` |
| `asa_create_adgroup` | New ad group | `campaign_id` + targeting/bid fields |
| `asa_update_campaign` | Pause/resume, budget | `campaign_id` |
| `asa_update_adgroup` | Bid, status | `campaign_id`, `ad_group_id` |

## Keywords

| MCP Tool | Purpose | Key args |
|----------|---------|----------|
| `asa_list_targeting_keywords` | Current keywords | `campaign_id`, `ad_group_id` |
| `asa_create_targeting_keywords` | Bulk add | `campaign_id`, `ad_group_id`, `keywords` |
| `asa_update_targeting_keywords` | Bid/status updates | `campaign_id`, `ad_group_id` |
| `asa_delete_targeting_keywords` | Remove | `campaign_id`, `ad_group_id` |
| `asa_targeting_keyword_recommendations` | New keyword ideas | `campaign_id`, `ad_group_id` |
| `asa_bid_recommendations` | Suggested bids | `campaign_id`, `ad_group_id`, `keywords` |

## Negatives

| MCP Tool | Purpose | Key args |
|----------|---------|----------|
| `asa_report_search_terms` | Wasted spend terms | `campaign_id` |
| `asa_create_campaign_negative_keywords` | Block terms | `campaign_id`, `keywords` |
| `asa_list_campaign_negative_keywords` | List campaign negatives | `campaign_id` |
| `asa_create_adgroup_negative_keywords` | Ad group negatives | `campaign_id`, `ad_group_id`, `keywords` |
| `asa_list_adgroup_negative_keywords` | List ad group negatives | `campaign_id`, `ad_group_id` |

## Reports & ROAS

| MCP Tool | Purpose |
|----------|---------|
| `asa_report_keywords` | Keyword performance (`campaign_id`) |
| `asa_report_search_terms` | Search term report (`campaign_id`) |
| `asa_profitability` | ASA spend × RC revenue join |
| `asa_playbook_status` | Integration readiness |
| `asa_admaxxing_recommendations` | Scale/pause/negative actions |
| `asa_review_country_gate` | Block spend if rating too low (`app_id`, `min_rating`) |

### Profitability Params (MCP)

- `level`: `keyword`, `campaign`, `adgroup`, `search_term`, `country`
- `days` or `from`/`to`
- `rc_key` required (or stored RevenueCat credentials)
- Optional `country` ISO filter
- Requires ASA credentials + `X-RC-Key` or stored RevenueCat credentials
