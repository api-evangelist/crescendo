---
name: Process a VOC recording with Crescendo
description: Upload an audio recording for voice-of-customer processing and poll the job to completion.
api: openapi/crescendo-platform-openapi-original.json
operations: [uploadVocRecording, getVocJobStatus]
---

# Process a VOC recording with Crescendo

Base URL: `https://platform.crescendo.ai`. Auth: `Authorization: Bearer $CRESCENDO_API_KEY` (tenant-scoped).

## Steps

1. **Upload the recording** — `uploadVocRecording`
   `POST /api/v1/voc/tenants/{tenantId}/recording` as `multipart/form-data`:
   - `audio` (or `file`): an mp3/wav binary.
   - `metadata`: a JSON string that MUST include an `id`.
   Returns `202 Accepted` with `{ "id": "<jobId>" }` and starts asynchronous processing.
2. **Poll the job** — `getVocJobStatus`
   `GET /api/v1/voc/tenants/{tenantId}/recording/{jobId}/status`
   Returns `{ id, status, started, finished, file, recordingId, result, error }`.
3. **Loop** until `status` is `completed` (read `result`) or `failed` (read `error`); `started` is the initial state.

## Rules
- The upload is asynchronous — never block on the POST; always poll the returned job id.
- Errors use `{ "code", "message" }` (see errors/crescendo-problem-types.yml).
