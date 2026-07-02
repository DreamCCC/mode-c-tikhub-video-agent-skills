---
name: tikhub-video-search
description: Searches all-web video materials through TikHub for Mode C. Use when the user asks for external video footage, TikTok/Douyin/YouTube/Bilibili/Xiaohongshu/Kuaishou/Weibo/Instagram/Twitter/Xigua/WeChat videos, download links, preview links, source clips, or director-style story/script video material planning.
---

# TikHub Video Search

## Mandatory Trigger

Use this skill when the user wants video materials from outside the official NBA media library, including:

- direct requests such as `找一些约基奇低位单打的视频`
- broad requests such as `全网找库里关键三分素材`
- director-style requests with story, script, visual style, emotional tone, or segment descriptions
- requests that mention TikHub, Mode C, B-mode-like material search, preview links, download links, source links, or external footage

## Required References

Read these repository files before answering:

- `tikhub-video-search/SKILL.md`
- `tikhub-video-search/intent-routing.md`
- `tikhub-video-search/tikhub-api.md`
- `tikhub-video-search/output-contract.md`
- `tikhub-video-search/quality-checklist.md`

Read `tikhub-video-search/examples.md` when a concrete example helps.

## Required Output

Return valid JSON only, following `tikhub-video-search/output-contract.md`.

Do not return Markdown tables, Mermaid diagrams, or prose outside JSON.
