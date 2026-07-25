---
name: Manage Crescendo provisioning documents
description: Read, replace, and merge tenant-scoped provisioning documents and their current version.
api: openapi/crescendo-platform-openapi-original.json
operations: [listProvisioningCollection, getProvisioningDocument, postProvisioningDocument, putProvisioningDocument, getProvisioningVersionCurrent, putProvisioningVersionCurrent]
---

# Manage Crescendo provisioning documents

Base URL: `https://platform.crescendo.ai`. Auth: `Authorization: Bearer $CRESCENDO_API_KEY`.
All routes are tenant-scoped; the key must be authorized for `{tenantId}` or the call returns `403 PermissionDenied`.

## Steps

1. **List a collection** — `listProvisioningCollection`
   `GET /api/v1/provisioning/tenants/{tenantId}/{collection}` returns an array of `{ id, data }`.
2. **Read one document** — `getProvisioningDocument`
   `GET /api/v1/provisioning/tenants/{tenantId}/{collection}/{docId}` returns the document object (404 if missing).
3. **Create or fully replace** — `postProvisioningDocument`
   `POST .../{collection}/{docId}` — POST = replace (creates if absent, otherwise overwrites). Returns 200/201.
4. **Merge fields into an existing document** — `putProvisioningDocument`
   `PUT .../{collection}/{docId}` — PUT = merge; returns `404` if the document does not exist.
5. **Versioned resources** — read `getProvisioningVersionCurrent` and merge with `putProvisioningVersionCurrent`
   on `.../{docId}/versions/current`.

## Rules
- Writes auto-stamp `updated` and `updatedBy` — do not set these yourself.
- Choose POST for full replacement, PUT for partial merge. There is no idempotency-key header.
- Errors use `{ "code", "message" }` (see errors/crescendo-problem-types.yml).
