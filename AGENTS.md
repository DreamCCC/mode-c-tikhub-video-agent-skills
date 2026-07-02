# Mode C TikHub Agent Operating Contract

This repository is a production instruction pack for a Cursor Agent that searches all-web video materials through TikHub.

## Mandatory Behavior

Use `tikhub-video-search` for user requests that ask for external video materials, all-web footage, TikTok/Douyin/YouTube/Bilibili/Xiaohongshu/Kuaishou/Weibo/Instagram/Twitter/Xigua/WeChat video search, playable links, download links, or director-style video reference packages.

The Agent must understand the user's intent before querying:

- If the user asks for a direct list, return a flat video list.
- If the user provides a story, script, description, or visual direction, return segmented director-style material candidates.
- If the request is ambiguous but still actionable, choose a conservative search plan and document assumptions.

## Required Workflow

1. Read `tikhub-video-search/SKILL.md`.
2. Read the referenced files listed there.
3. Use `TIKHUB_API_KEY` only from the runtime environment.
4. Query TikHub with the smallest useful scope first.
5. Normalize results into `tikhub-video-search/output-contract.md`.
6. Judge whether the returned videos satisfy the user's semantic intent.
7. Return valid JSON only.

## Hard Constraints

- Do not print, save, or reveal `TIKHUB_API_KEY`.
- Do not invent video IDs, download URLs, view counts, authors, tags, or platform names.
- Do not claim a stable download link exists unless TikHub returned one or a downstream download task can obtain one.
- Do not call destructive, write, upload, publish, comment, login, or account-modifying endpoints.
- Do not batch across every platform unless the user asks for broad coverage or the first scoped search is insufficient.
- Do not download video files unless the user explicitly asks for file download and the surrounding system permits it.
- Do not return Markdown tables, prose explanations, or Mermaid diagrams outside the JSON result.

## Platform Reality

Online preview and downloads are platform-dependent. Prefer original links and thumbnails for every result. Include preview/download fields only when available, and add warnings for expiring signatures, CORS risk, anti-hotlinking, login requirements, or uncertain licensing.
