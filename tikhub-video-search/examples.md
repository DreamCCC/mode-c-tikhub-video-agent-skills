# Examples

## Flat Search

User:

```text
帮我全网找 8 条约基奇低位单打的视频素材，优先 YouTube 和 B 站。
```

Expected behavior:

- `result_type`: `flat_search`
- Platforms: YouTube, Bilibili, then optionally TikTok/Douyin if results are weak.
- Query variants:
  - `Nikola Jokic low post highlights`
  - `Jokic post moves`
  - `约基奇 低位 单打`
- Return ranked material cards with original URLs, thumbnails, descriptions, and preview/download status.

## Director Plan

User:

```text
我要做一个库里最后一舞的短视频。开头要宿命感，中段要巅峰三分和逆风回应，结尾要有落寞和传承感。帮我找外网素材。
```

Expected behavior:

- `result_type`: `director_plan`
- Segments:
  - `hook`: last dance / emotional entrance / aging superstar
  - `peak`: deep threes / championship moments / crowd reaction
  - `conflict`: losses / injuries / late-game pressure
  - `ending`: farewell feeling / legacy / next generation
- Return 3-8 candidate materials per segment.
- Warn that "最后一舞" is a narrative framing unless tied to a verified retirement/source claim.

## Hybrid

User:

```text
按我这段脚本分段找素材，同时给我一个全局候选池，方便编辑自己替换。
```

Expected behavior:

- `result_type`: `hybrid`
- Return `segments[]` with per-segment materials.
- Return global `materials[]` deduplicated by `material_id`.
- Include `used_in_segments` or segment references in each material when useful.

## Download Metadata Request

User:

```text
找 5 条 NBA highlights 视频，并告诉我哪些能下载。
```

Expected behavior:

- Search first.
- For shortlisted candidates, call detail/stream endpoints only as needed.
- Set `download.status` explicitly:
  - `available` when a usable URL is returned.
  - `available_on_request` when a downstream call can request it but was not called.
  - `restricted` when platform restrictions are likely.
  - `unknown` when TikHub did not provide enough information.
