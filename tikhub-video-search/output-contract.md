# Output Contract

Return one JSON object. Do not wrap it in markdown fences unless the caller explicitly requests markdown.

## Success Shape

```json
{
  "success": true,
  "result_type": "flat_search",
  "topic": "用户原始需求",
  "interpreted_intent": "Agent 对用户真实检索意图的中文概括",
  "source_context": "用户提供的故事、脚本或描述，可为空",
  "platform_scope": ["youtube", "bilibili", "douyin"],
  "language": "zh-CN",
  "materials": [
    {
      "material_id": "youtube:WXMjsJuBs_s",
      "platform": "youtube",
      "platform_video_id": "WXMjsJuBs_s",
      "title": "视频标题",
      "description": "视频简介或摘要",
      "tags": ["NBA", "highlights"],
      "author": {
        "name": "频道或作者名",
        "id": "频道或作者 ID",
        "url": "作者主页 URL"
      },
      "duration_s": 710,
      "published_at": "2026-07-01T00:00:00Z",
      "view_count": 5797,
      "thumbnail_url": "https://example.com/thumb.jpg",
      "original_url": "https://www.youtube.com/watch?v=...",
      "preview": {
        "status": "available",
        "type": "embed",
        "url": "https://www.youtube.com/embed/...",
        "expires_at": null
      },
      "download": {
        "status": "available_on_request",
        "url": null,
        "expires_at": null,
        "format": null,
        "quality": null
      },
      "tikhub": {
        "endpoint": "/api/v1/youtube/web_v2/get_general_search_v2",
        "request_id": "TikHub request id",
        "raw_item_path": "data.videos[0]",
        "cache_url": "https://cache.tikhub.io/..."
      },
      "scores": {
        "semantic_score": 0.92,
        "visual_score": 0.84,
        "freshness_score": 0.7,
        "source_score": 0.8,
        "download_score": 0.5,
        "overall_score": 0.78
      },
      "selection_reason": "为什么这条素材符合用户需求",
      "risks": ["copyright_unknown"]
    }
  ],
  "queries": [
    {
      "purpose": "YouTube 关键词搜索",
      "platform": "youtube",
      "endpoint": "/api/v1/youtube/web_v2/get_general_search_v2",
      "params": {
        "keyword": "NBA highlights",
        "type": "video"
      },
      "status_code": 200,
      "request_id": "TikHub request id",
      "result_count": 10,
      "truncated": false,
      "next_page_token": "continuation token if any"
    }
  ],
  "assumptions": [],
  "warnings": [],
  "quality_checks": {
    "intent_classified": true,
    "all_materials_have_original_url": true,
    "all_urls_from_tikhub_or_platform": true,
    "no_fabricated_metadata": true,
    "preview_download_status_explicit": true,
    "weak_results_explained": true
  }
}
```

## Director Plan Shape

Use this shape when `result_type` is `director_plan` or `hybrid`:

```json
{
  "success": true,
  "result_type": "director_plan",
  "topic": "用户原始需求",
  "interpreted_intent": "中文概括",
  "source_context": "故事、脚本或描述",
  "platform_scope": ["youtube", "douyin", "bilibili"],
  "segments": [
    {
      "segment_id": "s1",
      "role": "hook",
      "story_or_voiceover": "用户给出的或 Agent 整理出的该段文本",
      "visual_intent": "该段需要什么画面",
      "search_queries": ["Stephen Curry last dance highlights"],
      "materials": ["youtube:abc123", "bilibili:BV..."],
      "coverage_note": "这些素材如何覆盖该段视觉需求",
      "warnings": []
    }
  ],
  "materials": [],
  "queries": [],
  "assumptions": [],
  "warnings": [],
  "quality_checks": {}
}
```

`materials[]` uses the same material schema from the success shape.

## Failure Shape

```json
{
  "success": false,
  "result_type": "flat_search",
  "topic": "用户原始需求",
  "blocking_reason": "无法完成的原因",
  "partial_findings": [],
  "queries": [],
  "assumptions": [],
  "warnings": [],
  "suggested_next_questions": ["需要用户补充的问题"]
}
```

## Download Resolve Shape

Use this shape when `result_type` is `download_resolve`:

```json
{
  "success": true,
  "result_type": "download_resolve",
  "material_id": "youtube:WXMjsJuBs_s",
  "platform": "youtube",
  "platform_video_id": "WXMjsJuBs_s",
  "original_url": "https://www.youtube.com/watch?v=...",
  "download": {
    "status": "available",
    "url": "https://signed-or-direct-download-url",
    "expires_at": "2026-07-02T12:00:00Z",
    "format": "mp4",
    "quality": "720p"
  },
  "preview": {
    "status": "available",
    "type": "stream",
    "url": "https://signed-or-direct-preview-url",
    "expires_at": "2026-07-02T12:00:00Z"
  },
  "tikhub": {
    "endpoint": "/api/v1/youtube/web_v2/get_signed_stream_url",
    "request_id": "TikHub request id",
    "params": {
      "video_id": "WXMjsJuBs_s"
    }
  },
  "warnings": ["signed_url_may_expire"]
}
```

If no usable URL is available:

```json
{
  "success": false,
  "result_type": "download_resolve",
  "material_id": "youtube:WXMjsJuBs_s",
  "platform": "youtube",
  "platform_video_id": "WXMjsJuBs_s",
  "original_url": "https://www.youtube.com/watch?v=...",
  "download": {
    "status": "restricted",
    "url": null,
    "expires_at": null,
    "format": null,
    "quality": null
  },
  "blocking_reason": "TikHub did not return a direct or signed downloadable URL.",
  "tikhub": {},
  "warnings": []
}
```

## Field Rules

- `material_id` must be deterministic: `<platform>:<platform_video_id>` when possible.
- `original_url` is required for every successful material.
- `preview.status` must be one of `available`, `available_on_request`, `unavailable`, `unknown`, or `restricted`.
- `download.status` must be one of `available`, `available_on_request`, `unavailable`, `unknown`, or `restricted`.
- `download.url` must be null unless TikHub returned a direct/signed URL and the caller asked for download information.
- `tikhub.endpoint` and `queries[]` must identify the endpoints used.
- `scores.overall_score` should reflect actual fit, not just keyword overlap.
- `risks` should include `copyright_unknown`, `signed_url_may_expire`, `cors_risk`, `login_required`, `low_relevance`, or `metadata_incomplete` when applicable.
