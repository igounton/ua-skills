# Appeeky RevenueCat

Subscription metrics and attribution for paid growth ROAS analysis.

**Docs:** [docs.appeeky.com/docs/revenuecat-overview](https://docs.appeeky.com/docs/revenuecat-overview)

MCP tool arguments use **snake_case**. REST path/query fields use camelCase or the path segment.

## Headers

| Header | Required | Description |
|--------|----------|-------------|
| `X-RC-Key` | Yes | RevenueCat secret key (`sk_xxx`) |
| `X-RC-Project` | No | Project ID if multiple projects |

MCP: pass `rc_key` and `rc_project` on each tool (optional if stored in Appeeky Connect).

## Endpoints

| Method | Path | MCP Tool |
|--------|------|----------|
| `GET` | `/v1/revenuecat/overview` | `rc_overview` |
| `GET` | `/v1/revenuecat/charts/:chartName` | `rc_chart` (`chart_name`, `start_date`, `end_date`) |
| `GET` | `/v1/revenuecat/attribution-summary` | `rc_attribution_summary` |
| `GET` | `/v1/revenuecat/customers/:id/attributes` | `rc_customer_attributes` |

Dedicated chart tools: `rc_mrr`, `rc_revenue`, `rc_active_subscriptions`, `rc_churn`, `rc_chart_options`.

### rc_chart (MCP)

```
rc_chart
  rc_key: "<sk_xxx>"
  rc_project: "<proj_xxx>"
  chart_name: "revenue"
  start_date: "2026-07-25"
  end_date: "2026-08-22"
  currency: "USD"
```

`chart_name` values include `revenue`, `mrr`, `churn`, `actives`, `trial_conversion`. There is no `days` argument — use `start_date` / `end_date`.

## Stored Credentials (Connect)

| Endpoint | Purpose |
|----------|---------|
| `POST /v1/connect/revenuecat/credentials` | Save RC key in Vault |
| `GET /v1/connect/revenuecat/credentials/status` | Connection status |
| `GET /v1/connect/revenuecat/attribution-summary` | Aggregate by media source/campaign/keyword |

## Key Metrics

| ID | Description |
|----|-------------|
| `mrr` | Monthly Recurring Revenue |
| `active_subscriptions` | Paying subscribers |
| `active_trials` | Active trials |
| `revenue` | Last 28 days revenue |
| `new_customers` | Last 28 days new customers |

## Attribution Fields

Parsed from customer attributes: `mediaSource`, `campaign`, `adGroup`, `ad`, `keyword`, `creative`.

Use with `asa_profitability` for Apple Search Ads ROAS.
