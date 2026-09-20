---
name: movehome-org-syndicate-listings
description: Push a listing agent's inventory into MoveHome.org through the OAuth2-gated RAIA Portal Feed API — mint a token, upsert and withdraw listings idempotently by your own reference, reconcile a branch, and poll enquiries by cursor.
api: openapi/movehome-org-raia-portal-feed-openapi.yaml
base_url: https://movehome.org/api/raia/portal/v1
operations:
  - getHealth
  - upsertListing
  - getListing
  - deleteListing
  - listBranchListings
  - listBranchEnquiries
  - getBranchPerformance
  - requestPremiumListingActivation
  - getPremiumListingActivation
method: generated
generated: '2026-09-19'
grounding: Every operationId above exists verbatim in openapi/movehome-org-raia-portal-feed-openapi.yaml (the RAIA Protocol's published contract, which MoveHome implements — see overlays/movehome-org-raia-portal-feed-overlay.yaml for the live servers/tokenUrl binding). Base URLs, scopes, idempotency, limits and error rules are quoted from the provider's docs/raia-portal-feed-api.md; the token endpoint and 401 envelope were observed live on 2026-09-19. Credentials are issued out-of-band and were not used.
---

# Syndicate listings into MoveHome.org

You are a CRM or agency pushing property listings IN. This is the write side of MoveHome; buyers' agents read the result through A2A/MCP. Health first: `GET /healthz` (`getHealth`, no auth) returned `{"status":"ok","version":"0.1.0","service":"raia-portal-feed-api"}`.

## 0. Credentials and token

Credentials (`client_id`, `client_secret`) come from the MoveHome operator (admin@movehome.org); the secret is shown once. Mint a token with OAuth2 client credentials:

```
POST https://movehome.org/oauth/token
Authorization: Basic base64(client_id:client_secret)   (or form fields client_id / client_secret)
grant_type=client_credentials&scope=feed.read feed.write products.write
```

You get `{ access_token, token_type: "Bearer", expires_in: 3600, scope }`. Scopes are intersected with what your credential allows: `feed.read` (reads), `feed.write` (PUT/DELETE listings), `products.write` (activations). Cache the token for the hour; the token endpoint is capped at 10/min. A missing credential returns 401 `application/problem+json` with `WWW-Authenticate: Bearer realm="raia-portal-feed"`.

Send `Authorization: Bearer <token>` on every call. Optional `X-RAIA-Branch-Id: <branch>` scopes a call to a branch; otherwise your credential's default branch applies.

## 1. Upsert — `upsertListing` (`PUT /listings/{reference}`), scope `feed.write`

`{reference}` is YOUR stable id (`^[A-Za-z0-9_-]{1,100}$`, unique per branch). Body ≤ 1 MB, `kind` `residential` or `commercial`, with `transaction_type` (`SALES` | `LETTINGS`), `status`, `property_type`, `headline`, price fields and `address`.

- New reference → `201` `{ action: "CREATED", public_card_url, version: 1 }`
- Changed body → `200` `UPDATED`
- Identical body → `200` `NO_CHANGE` — this is the idempotency contract: re-sending is safe.
- `400` → fix the fields named in `validation_errors[]`; `409` → the reference already exists under a different branch of yours.

Keep `public_card_url` (`https://movehome.org/property/{raia_id}`); it is the buyer-facing page and the `raia_id` inside it is what A2A/MCP callers will use.

## 2. Read back — `getListing` (`GET /listings/{reference}`), scope `feed.read`

Returns the stored listing with `version`, `created_at`, `updated_at`. `404` means not found or not yours.

## 3. Withdraw — `deleteListing` (`DELETE /listings/{reference}`), scope `feed.write`

DELETE requires a body: `{ "removal_reason": "LET_BY_US" }` (one of `SOLD_BY_US SOLD_BY_ANOTHER_AGENT LET_BY_US LET_BY_ANOTHER_AGENT WITHDRAWN_FROM_MARKET LOST_INSTRUCTION REMOVED`, optional `note`). Deleting an already-removed listing still returns `200` (idempotent). The docs do not say whether a later PUT of the same reference restores the same card — treat withdrawal as final and re-list deliberately.

## 4. Reconcile nightly — `listBranchListings` (`GET /branches/{branch_id}/listings`)

Filters `transaction_type`, `status`, `updated_since`; pages by `page` / `per_page` (docs example 50; schema max 200); response `{ meta: { page, per_page, total }, listings[] }`. Diff against your source of truth and PUT/DELETE the differences.

## 5. Poll leads — `listBranchEnquiries` (`GET /branches/{branch_id}/enquiries`)

Cursor pagination: pass the previous response's `next_cursor` as `since_enquiry_id`, `limit` up to 100. Each enquiry carries `enquiry_id`, `listing_reference`, `received_at`, `source` and the enquirer block. There is no push to CRMs; poll.

## 6. Performance — `getBranchPerformance` (`GET /branches/{branch_id}/performance`)

`from` and `to` are required and the window is ≤ 28 days (`400` otherwise); optional `portal`. Returns totals and `by_day[]` of impressions, detail_views, click_throughs, phone_reveals, brochure_downloads, enquiries.

## 7. Optional promotion — `requestPremiumListingActivation` / `getPremiumListingActivation`, scope `products.write` / `feed.read`

`POST /products/premium-listings` with `customer_listing_id` (your reference) or `listing_id`, plus `highlights[]`; `201` with `status: "PENDING"`; poll `GET /products/premium-listings/{id}` until `ACTIVE | EXPIRED | REJECTED | CANCELLED`. No cancel operation exists — do not promise one. MoveHome publishes no price for these products.

## Runtime rules

- 60 requests/min per credential per endpoint group; read `X-RateLimit-Remaining`; on `429` wait `Retry-After` seconds (the window is a fixed UTC minute).
- Every error is `application/problem+json` (`type` under `https://movehome.org/errors/`, `trace_id` also in `X-Trace-Id`) — quote `trace_id` when writing to the operator.
- `403` means your token lacks the scope; re-mint with it rather than retrying.
