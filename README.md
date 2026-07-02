# Mode C TikHub Video Agent Skills

Cursor Agent instruction pack for Mode C: flexible all-web video material search through TikHub.

## Skill

- `tikhub-video-search`: interprets natural-language video material requests, calls TikHub, evaluates result fit, and returns normalized video material cards or director-style segmented material plans.

## Runtime Secret

The remote Agent runtime must provide:

```text
TIKHUB_API_KEY
```

Never commit real keys, cookies, raw downloaded videos, cache dumps, or private user data.

## What This Agent Produces

- Direct search results when the user asks for a list of videos.
- Script/director-style material plans when the user provides a story, description, visual direction, or segment intent.
- TikHub source metadata, original URLs, preview information, and download information when safely available.
- Warnings when a platform cannot provide stable preview/download links.

The Agent does not synthesize voice, edit videos, upload media, run FFmpeg, or create final videos.

## Structure

```text
mode-c-tikhub-video-agent-skills/
├── AGENTS.md
├── README.md
├── .cursor/skills/tikhub-video-search/SKILL.md
└── tikhub-video-search/
    ├── SKILL.md
    ├── tikhub-api.md
    ├── intent-routing.md
    ├── output-contract.md
    ├── quality-checklist.md
    └── examples.md
```

## Smoke Prompts

Read-only:

```text
Read tikhub-video-search/SKILL.md and summarize the output contract. Do not call TikHub.
```

Low-cost live query:

```text
帮我找 5 条 NBA highlights 的 YouTube 视频素材，只返回 JSON，不要拉下载链接。
```
