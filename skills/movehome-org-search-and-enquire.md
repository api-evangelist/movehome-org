---
name: movehome-org-search-and-enquire
description: Find a property on MoveHome.org through the anonymous A2A agent (or its read-only MCP twin), inspect one listing, and — only with the user's explicit consent — submit an enquiry that reaches a real estate agent.
api: a2a/movehome-org-agent-card.json
surfaces:
  - https://movehome.org/api/a2a (A2A 0.3.0, JSON-RPC 2.0)
  - https://movehome.org/mcp (MCP 2025-06-18, read-only)
operations:
  - search_properties
  - get_property
  - create_enquiry
method: generated
generated: '2026-09-19'
grounding: The three skill ids above are the skills[] of the live agent card (a2a/movehome-org-agent-card.json); search_properties and get_property are also the two tools of mcp/movehome-org-mcp-tools.json. Parameter names, enums, limits and error behaviour are quoted from https://movehome.org/skills.md and were exercised live on 2026-09-19 (read skills only). The provider's own skills document is saved verbatim as skills/movehome-org-skills.md; read it first.
---

# Search MoveHome.org and enquire on a listing

Free, anonymous, no key. Every call is `POST https://movehome.org/api/a2a` with a JSON-RPC 2.0 `message/send` whose message holds ONE DataPart `{"skill": "<id>", "params": {...}}`. The reply is an A2A Task: read `result.status.state` first, then `result.artifacts[0].parts[0].data`. Budget: 60 requests/min per IP (`X-RateLimit-*` headers on every response).

## 1. Search — `search_properties` (read-only)

Params, all optional, strict schema (unknown keys are rejected): `un_locode` (5-char UN/LOCODE, `GBLON`, `GBMNC`), `service_type` (`long_term` | `short_term` | `sale`), `property_type` (`flat` | `house` | `studio` | `commercial` | `land` | `other`), `bedrooms_min` / `bedrooms_max` (0–50), `rent_pcm_max`, `asking_price_max`, `features[]` (≤20, listing must contain all), `limit` (1–50, default 24), `offset`.

Artifact `search_results` → `{ total, count, offset, limit, listings[] }`. Page with `offset`. Observed live: `GBLON` returned `total: 21`.

If you are on the MCP server instead, call tool `search_properties` — same filters minus `bedrooms_max`, `features` and `offset` (it cannot page); switch to A2A when you need more than one page.

## 2. Inspect — `get_property` (read-only)

Param `raia_id` (required, from a search result; format `prop-gb-rlf-000031`). Artifact `property` → `{ listing }` in the same RAIA card shape (price, location, features, media). A wrong id does NOT raise a JSON-RPC error: you get HTTP 200 and a Task with `status.state: "failed"` and text like `No public listing found for prop-gb-none-00000000.` Check `status.state` before trusting `artifacts`.

## 3. Enquire — `create_enquiry` (WRITE, irreversible)

Stop and confirm with the user first. The provider's text: "This records an enquiry and forwards it to the source estate agent (a real human gets it). Only call it with the user's explicit consent and their real contact details." There is no cancel or withdraw skill.

Params: `raia_id` (required); `enquirer.name` (required, 1–200), `enquirer.email` (required, valid), `enquirer.phone` (optional), `enquirer.preferred_contact` (`email` | `phone` | `whatsapp`); `message` (required, 1–2000); optional `viewing_request.preferred_dates[]` (1–3 ISO-8601 dates) and `viewing_request.party_size` (1–50). Note the NESTED `enquirer` object.

Artifact `enquiry_receipt` → `{ enquiry_id, status: "received" }`. Keep `enquiry_id`; it is the only handle you get.

Retry rules: this skill is capped at about 5/min per IP, the same email + listing within ~10 minutes is dropped as a duplicate, and there is a per-email hourly cap. Do not retry a timeout blindly — a retry outside the duplicate window sends a second real lead. Send once, record the receipt, and surface any `failed` Task text to the user.

## Errors you will see

- Skill-level (bad params, unknown listing, unknown skill, rate limited): HTTP 200, Task `status.state: "failed"`, reason in `status.message.parts[0].text`.
- Protocol-level: JSON-RPC `error` — `-32700` malformed JSON, `-32601`/`-32602` unknown method or params, `-32004` you used `message/stream` (streaming is off; use `message/send`), `-32001` `tasks/get` (tasks are not persisted — never poll), `-32603` rate limit / internal.

## Do not

- Do not call `message/stream` or `tasks/get`; both fail by design.
- Do not invent a `raia_id`; always take it from a search result.
- Do not fill `enquirer.email` with a placeholder — it is forwarded to a human.
