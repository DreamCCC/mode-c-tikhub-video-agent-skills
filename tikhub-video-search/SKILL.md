---
name: tikhub-video-search
description: Searches all-web video materials through TikHub for Mode C. Use for natural-language external video search, cross-platform social video retrieval, preview/download metadata, and director-style segmented material planning.
---

# TikHub Video Search

## Goal

Given a natural-language request, find relevant external video materials through TikHub and return a structured result. The output is a material plan only: do not synthesize voice, edit video, upload media, run FFmpeg, or create final media files.

Support two user modes:

- `flat_search`: the user directly asks for videos or a video list.
- `director_plan`: the user provides a story, script, visual description, emotional direction, or segmented creative intent.

## Required References

Read these files before doing the task:

- `intent-routing.md` for deciding request mode and search plan.
- `tikhub-api.md` for TikHub authentication, platform coverage, endpoint preferences, and safety.
- `output-contract.md` for the final JSON shape.
- `quality-checklist.md` before returning.
- `examples.md` when a concrete example is useful.

## Workflow

1. Parse the user request.
   - Extract topic, entities, target platforms, language, region, duration, time range, tone, story beats, visual themes, and result count.
   - If the user references prior script/story text, preserve it as `source_context`.
   - If the user does not specify platforms, choose platform groups based on intent.

2. Choose a task mode.
   - Use `flat_search` for direct requests.
   - Use `director_plan` when the request contains story/script/description/segment needs.
   - Use `hybrid` only when both a full list and segment-specific candidates are useful.

3. Build a conservative TikHub query plan.
   - Start with 1-4 platforms most likely to contain the requested material.
   - Use broader all-platform search only if the user explicitly asks for full-web coverage or initial results are weak.
   - Avoid expensive detail/download calls until search results look relevant.

4. Call TikHub.
   - Read `TIKHUB_API_KEY` from the runtime environment.
   - Use `Authorization: Bearer $TIKHUB_API_KEY`.
   - Prefer search endpoints first, then detail endpoints for shortlisted candidates, then preview/download endpoints only when needed.
   - Record endpoint path, platform, request parameters, status, TikHub `request_id`, and pagination token when returned.

5. Normalize results.
   - Convert platform-specific fields into the material schema in `output-contract.md`.
   - Preserve TikHub metadata and original platform URLs.
   - Include tags, description, author, duration, thumbnails, preview URLs, and download URLs only when returned or safely derived.

6. Evaluate fit.
   - Score semantic relevance, visual usefulness, freshness, source quality, duration fit, and platform reliability.
   - For `director_plan`, assign 3-8 candidates per segment by default.
   - If results are weak, broaden the query once and document the fallback.

7. Return JSON only.
   - Include assumptions, warnings, and query trace.
   - Include preview/download limitations.
   - Never include secrets.

## Hard Rules

- Do not fabricate video IDs, titles, authors, tags, view counts, durations, URLs, preview URLs, or download URLs.
- Do not print, persist, or reveal `TIKHUB_API_KEY`.
- Do not call account-mutating, upload, publish, comment, login, or destructive endpoints.
- Do not download large media files unless the user explicitly asks and the runtime allows it.
- Do not claim copyright clearance. Report source and licensing uncertainty.
- Do not return `success=true` if no useful videos were found. Return `success=false` with partial findings.
- Do not expose signed stream URLs if the caller asked only for search metadata; mark them as `available_on_request`.
