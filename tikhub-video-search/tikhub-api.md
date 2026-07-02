# TikHub API Guidance

## Authentication

Use the runtime environment variable:

```text
TIKHUB_API_KEY
```

Send it as:

```text
Authorization: Bearer $TIKHUB_API_KEY
Accept: application/json
```

Never print, store, or return the token.

## Base URL

Use:

```text
https://api.tikhub.io
```

The OpenAPI document is available at:

```text
https://api.tikhub.io/openapi.json
```

## Endpoint Strategy

Use a staged approach:

1. Search endpoint.
2. Detail endpoint for shortlisted results.
3. Stream/download endpoint only when the user asked for preview/download or when the output contract requires availability.

Do not call download/stream endpoints for every search result by default.

## Useful Platform Endpoints

Prefer these endpoint families when available:

- YouTube search: `/api/v1/youtube/web_v2/get_general_search_v2`
- YouTube Shorts search: `/api/v1/youtube/web_v2/get_shorts_search_v2`
- YouTube streams: `/api/v1/youtube/web_v2/get_video_streams_v2`
- YouTube signed stream: `/api/v1/youtube/web_v2/get_signed_stream_url`
- TikTok video search: `/api/v1/tiktok/app/v3/fetch_video_search_result`
- TikTok detail: `/api/v1/tiktok/app/v3/fetch_one_video_by_share_url`
- Douyin video search: `/api/v1/douyin/search/fetch_video_search_v1`
- Douyin high quality URL: `/api/v1/douyin/web/fetch_video_high_quality_play_url`
- Bilibili search: `/api/v1/bilibili/web/fetch_general_search`
- Bilibili play URL: `/api/v1/bilibili/web/fetch_video_playurl`
- Xiaohongshu search: `/api/v1/xiaohongshu/app_v2/search_notes`
- Xiaohongshu video note detail: `/api/v1/xiaohongshu/app_v2/get_video_note_detail`
- Kuaishou search: `/api/v1/kuaishou/app/search_video_v2`
- Weibo search: `/api/v1/weibo/web_v2/fetch_advanced_search`
- Instagram Reels search: `/api/v1/instagram/v2/search_reels`
- Twitter/X search: `/api/v1/twitter/web/fetch_search_timeline`
- Xigua search: `/api/v1/xigua/app/v2/search_video`
- WeChat video search: `/api/v1/wechat_search/v2/fetch_search_videos`

If an endpoint fails or is discontinued, inspect OpenAPI and choose the nearest current endpoint in the same platform family.

## Cost And Rate Limits

TikHub requests may be billable and may return `429`.

Rules:

- Keep initial result count small, usually 5-10 per platform.
- Cache mentally within the run: do not repeat identical calls.
- Stop broadening after one weak-result retry unless the user explicitly asks for exhaustive search.
- Include `warnings` when rate limits or partial failures affect coverage.

## Preview And Download

Preview/download is platform-dependent:

- Always include `original_url` when available.
- Include `thumbnail_url` whenever available.
- Include `preview_url` only when it is safe for browser playback or embeddable.
- Include `download_url` only when TikHub returns a direct or signed URL and the user requested download metadata.
- If a signed URL expires, set `expires_at` or warn `signed_url_may_expire`.
- If CORS, anti-hotlinking, login, or platform restrictions are likely, set `preview_status` or `download_status` accordingly.

Never claim a clip is licensed for commercial reuse unless the source explicitly says so.
