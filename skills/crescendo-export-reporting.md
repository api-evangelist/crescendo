---
name: Export Crescendo conversations for reporting
description: Page through assistant and voice-of-customer conversation exports using cursor pagination.
api: openapi/crescendo-platform-openapi-original.json
operations: [listReportingBotConversations, listReportingVocConversations]
---

# Export Crescendo conversations for reporting

Base URL: `https://platform.crescendo.ai`. Auth: `Authorization: Bearer $CRESCENDO_API_KEY` (tenant-scoped).

## Steps

1. **Export assistant conversations** — `listReportingBotConversations`
   `GET /api/v1/reporting/tenants/{tenantId}/assistant/conversations`
2. **Export VOC conversations** — `listReportingVocConversations`
   `GET /api/v1/reporting/tenants/{tenantId}/voc/conversations`
3. **Scope the window** with `range`, or `from`/`to` (ISO 8601 or epoch ms). Set `limit` (default 1000, max 5000).
4. **Page** — the response envelope is `{ "total", "cursor": { "next" }, "data": [] }`.
   While `cursor.next` is non-null, repeat the request passing `cursor=<next>`.

## Rules
- Reporting rows contain no message/transcript content (metadata only).
- A `503 Unavailable` means the reporting backend is temporarily down — retry with backoff.
