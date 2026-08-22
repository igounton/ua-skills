# Tool Registry

Tools and integrations that UA Skills use for live campaign data and operations.

## Appeeky — Primary Integration

[Appeeky](https://appeeky.com) provides paid growth APIs via REST and MCP.

### Capability Matrix

| Capability | MCP Tool | Integration Guide |
|-----------|----------|-------------------|
| App ad creative generation | `generate_app_ad_creative`, `get_app_ad_creative_job` | [appeeky-ad-creatives.md](integrations/appeeky-ad-creatives.md) |
| Meta Ad Library (competitive intel) | `meta_ads_search`, `meta_ads_page_ads`, `meta_app_ad_intelligence` | [appeeky-meta-ads.md](integrations/appeeky-meta-ads.md) |
| Meta Marketing API (campaign CRUD) | `meta_ads_*` | [appeeky-meta-ads.md](integrations/appeeky-meta-ads.md) |
| TikTok Marketing API | `tiktok_ads_*` (performance via `tiktok_ads_performance`; no `tiktok_ads_report` MCP tool) | [appeeky-tiktok-ads.md](integrations/appeeky-tiktok-ads.md) |
| Apple Search Ads | `asa_*` | [appeeky-apple-search-ads.md](integrations/appeeky-apple-search-ads.md) |
| ASA × RevenueCat ROAS | `asa_profitability`, `asa_admaxxing_recommendations` | [appeeky-apple-search-ads.md](integrations/appeeky-apple-search-ads.md) |
| RevenueCat metrics | `rc_overview`, `rc_chart`, `rc_attribution_summary` | [appeeky-revenuecat.md](integrations/appeeky-revenuecat.md) |

### Skill → Tool Mapping

| Skill | Primary Tools |
|-------|---------------|
| `meta-ad-creative` | `generate_app_ad_creative`, `get_app_ad_creative_job` |
| `ad-creative-variants` | `generate_app_ad_creative` (multiple jobs) |
| `ad-creative-edit` | `generate_app_ad_creative` (`mode: edit`) |
| `ad-creative-to-meta` | `generate_app_ad_creative`, `meta_ads_upload_ad_image_from_url`, `meta_ads_create_app_install_draft` (or create campaign/adset/creative/ad) |
| `competitor-ad-teardown` | `meta_app_ad_intelligence`, `meta_ads_search` |
| `meta-campaign-setup` | `meta_ads_create_app_install_draft`, `meta_ads_create_campaign`, `meta_ads_create_adset`, `meta_ads_create_creative`, `meta_ads_create_ad` |
| `meta-campaign-audit` | `meta_ads_list_campaigns`, `meta_ads_insights` |
| `meta-budget-optimizer` | `meta_ads_insights`, `meta_ads_update_campaign`, `meta_ads_update_ad` |
| `tiktok-campaign-setup` | `tiktok_ads_create_campaign`, `tiktok_ads_create_adgroup`, `tiktok_ads_create_ad` |
| `tiktok-spark-ads` | `tiktok_ads_list_identities`, `tiktok_ads_create_ad`, `tiktok_ads_upload_video_from_url` |
| `tiktok-campaign-audit` | `tiktok_ads_performance` |
| `asa-roas-analysis` | `asa_profitability`, `rc_overview` |
| `asa-weekly-optimization` | `asa_report_keywords`, `asa_report_search_terms`, `asa_targeting_keyword_recommendations` |
| `asa-negative-keywords` | `asa_report_search_terms`, `asa_create_campaign_negative_keywords` |
| `asa-admaxxing` | `asa_playbook_status`, `asa_admaxxing_recommendations` |
| `subscription-snapshot` | `rc_overview`, `rc_mrr`, `rc_active_subscriptions` |
| `campaign-profitability` | `asa_profitability`, `meta_ads_insights`, `tiktok_ads_performance`, `rc_overview` |
| `cross-channel-performance` | `meta_ads_insights`, `tiktok_ads_performance`, `asa_profitability` |
| `mmp-setup` | — (configuration skill) |
| `ads-router` | — (router) |
| `app-ads-context` | `get_app` (optional) |
| `google-uac-campaign` | — (framework only) |
| `cross-channel-budget` | — (framework) |
| `tiktok-creative-strategy` | — (framework; optional `get_app` for listing context) |

## Other Tools

| Tool | Purpose | Integration |
|------|---------|-------------|
| **AppsFlyer / Adjust / Singular** | MMP for purchase signals | See `mmp-setup` skill; for SKAN deep dive see aso-skills `attribution-setup` |
| **aso-skills** | Organic ASO, listing optimization | [github.com/eronred/aso-skills](https://github.com/eronred/aso-skills) |
