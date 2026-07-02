# Quality Checklist

Before returning, verify every item that applies.

## Intent

- [ ] The request mode is `flat_search`, `director_plan`, or `hybrid`.
- [ ] The interpreted intent is written clearly in Chinese.
- [ ] Assumptions are documented instead of silently changing the request.
- [ ] Director-style requests are split into meaningful visual segments.

## TikHub Calls

- [ ] `TIKHUB_API_KEY` was read only from the runtime environment.
- [ ] No secret is printed, saved, or returned.
- [ ] Search endpoints were called before detail/download endpoints.
- [ ] Endpoint paths, params, status codes, request IDs, and result counts are recorded.
- [ ] Rate limits, failed platforms, or partial results are reflected in `warnings`.

## Material Quality

- [ ] Every successful material has `platform`, `title`, `original_url`, and `selection_reason`.
- [ ] Metadata is copied from TikHub/platform data, not invented.
- [ ] Obvious mismatches are rejected.
- [ ] Scores reflect semantic and visual fit.
- [ ] Tags are included only when returned or safely inferred from title/description.

## Preview And Download

- [ ] `preview.status` is explicit for every material.
- [ ] `download.status` is explicit for every material.
- [ ] Direct download URLs are included only when requested and available.
- [ ] Signed URL expiry, CORS, anti-hotlinking, login, and platform restrictions are warned when relevant.
- [ ] Copyright/licensing uncertainty is not hidden.

## Output

- [ ] Output is valid JSON only.
- [ ] `success=false` is used when no useful materials were found.
- [ ] `quality_checks` accurately describes what was verified.
- [ ] No Markdown table, prose explanation, or code fence is returned unless explicitly requested.
