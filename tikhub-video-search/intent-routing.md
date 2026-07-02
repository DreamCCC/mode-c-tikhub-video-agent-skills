# Intent Routing

## Modes

### flat_search

Use when the user asks directly for videos, source clips, highlights, downloads, or a video list.

Examples:

- `找 10 条库里超远三分视频`
- `全网搜约基奇低位脚步素材`
- `给我一些 NBA 交易爆料相关 YouTube 视频`

Return `result_type: "flat_search"` and a ranked `materials` list.

### director_plan

Use when the user provides a story, script, description, scene direction, emotional arc, visual plan, or B-mode-like reference package request.

Examples:

- `我要讲库里最后一舞，开头宿命感，中段高光，结尾落寞`
- `按这段脚本给每段找外网视频素材`
- `帮我做一个文班防守压迫感的视频素材包`

Return `result_type: "director_plan"` with `segments[]`, each containing matched materials.

### hybrid

Use only when the user asks for both a general list and segment-specific candidates.

Return both `segments[]` and a global `materials` list.

## Platform Selection Defaults

If the user does not specify platforms:

- NBA highlights, analysis, long-form recaps: YouTube, Bilibili.
- Short viral clips: TikTok, Douyin, Kuaishou, Xiaohongshu.
- Chinese social discussion/news: Weibo, WeChat Channels, Bilibili, Xiaohongshu.
- International reactions/news: YouTube, TikTok, Instagram, Twitter/X, Reddit.
- Exact Chinese creator references: Bilibili, Douyin, Xiaohongshu, Weibo.
- Official-ish clips or full highlights: YouTube first, then Bilibili.

When the user says "全网", search at least 4 platform groups unless a hard limit is provided.

## Query Expansion

Generate 2-5 query variants:

- Chinese query.
- English query.
- Player/team aliases and official names.
- Visual/action phrases such as `clutch three`, `poster dunk`, `low post`, `defense highlights`.
- Story phrases such as `last dance`, `comeback`, `collapse`, `revenge game`.

Keep expansion grounded in the user request. Do not invent facts.

## Relevance Judging

Score each candidate from 0 to 1:

- `semantic_score`: topic/entity match.
- `visual_score`: likely useful visual scene.
- `freshness_score`: date relevance when requested.
- `source_score`: author/channel/platform quality.
- `download_score`: preview/download availability and stability.

Reject obvious mismatches, unrelated compilations, clickbait with no relevant description, and results that only match a generic word.

## Clarification Policy

Ask a follow-up question only when the request cannot be acted on safely, for example:

- no topic or visual target is provided
- user requires a specific platform but does not give enough identifiers
- requested content is private, login-only, or likely unavailable

Otherwise make conservative assumptions and put them in `assumptions`.
