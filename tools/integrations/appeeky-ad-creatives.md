# Appeeky App Ad Creatives

Generate Meta-ready square ad creatives and paste-ready copy from a real App Store or Google Play listing.

**Docs:** [docs.appeeky.com/docs/app-ad-creatives](https://docs.appeeky.com/docs/app-ad-creatives)

## Naming

MCP tool arguments use **snake_case**. REST JSON bodies use **camelCase**. Job result fields (`result.ad.imageUrl`, `result.keyPoints`) are camelCase in both.

| MCP (tools) | REST (JSON) |
|-------------|-------------|
| `app_url` | `appUrl` |
| `app_id` | `appId` |
| `image_preset` | `imagePreset` |
| `generate_image` | `generateImage` |
| `key_points` | `keyPoints` |
| `edit_instruction` | `editInstruction` |
| `previous_ad` | `previousAd` |
| `job_id` | path `:jobId` |

## Endpoints

| Method | Path | Purpose |
|--------|------|---------|
| `POST` | `/v1/app-ad-creatives/generate` | Start async job (returns `jobId`, HTTP 202) |
| `GET` | `/v1/app-ad-creatives/jobs/:jobId` | Poll until `completed` or `failed` |

## MCP Tools

| Tool | Purpose | Key args |
|------|---------|----------|
| `generate_app_ad_creative` | Start job | `app_url` or `platform` + `app_id`, `mode`, `style`, `image_preset` |
| `get_app_ad_creative_job` | Poll job | `job_id` |

## Request Highlights

**MCP:**
```
generate_app_ad_creative
  app_url: "https://apps.apple.com/us/app/id123456789"
  mode: "generate"
  style: "ugc"
  image_preset: "person_holding_phone"
  angle: "problem-solution for busy founders"
  audience: "students and knowledge workers"
  quality: "medium"
```

**REST:**
```json
{
  "appUrl": "https://apps.apple.com/us/app/id123456789",
  "mode": "generate",
  "style": "ugc",
  "imagePreset": "person_holding_phone",
  "angle": "problem-solution for busy founders",
  "audience": "students and knowledge workers",
  "quality": "medium"
}
```

| Field | Values |
|-------|--------|
| `mode` | `analyze`, `generate`, `edit` |
| `style` | `ugc`, `professional`, `problem_solution`, `before_after`, `lifestyle` |
| `image_preset` / `imagePreset` | `branded_showcase`, `person_holding_phone`, `creator_testimonial`, `problem_solution_split`, `clean_app_store_mockup` |
| `generate_image` / `generateImage` | `false` for copy-only (1 API credit) |

## Polling

```
get_app_ad_creative_job
  job_id: "<job-id>"
```

Poll every 3–5 seconds until `status` is `completed` or `failed`.

| Status | Action |
|--------|--------|
| `queued` / `processing` | Keep polling |
| `completed` | Extract `result` |
| `failed` | Read `error`, retry or adjust inputs |

## Credits

| Mode | Cost |
|------|------|
| `analyze` / copy-only | 1 API credit |
| `generate` image | 1–5 creative credits (`low`/`medium`/`high`) |
| `edit` | 1–5 creative credits |

## Output Fields

- `result.keyPoints` — product positioning extracted from listing
- `result.ad.imageUrl` — 1024×1024 PNG
- `result.ad.copy` — `primaryText`, `headline`, `description`, `callToAction`

## Pipeline to Meta

Preferred one-shot (app install draft):

1. `generate_app_ad_creative` → poll `get_app_ad_creative_job`
2. `meta_ads_list_advertisable_applications` + `meta_ads_list_pages`
3. `meta_ads_create_app_install_draft` — campaign + ad set + creative + ad, all PAUSED

Manual pipeline:

1. `generate_app_ad_creative` → get `imageUrl` + copy
2. `meta_ads_upload_ad_image_from_url` (`image_url`) → `image_hash`
3. `meta_ads_create_creative` (`format: "app_install"`, required `name`)
4. `meta_ads_create_adset` (`destination_type: "APP"`) + `meta_ads_create_ad`
