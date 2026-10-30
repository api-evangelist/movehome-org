# Move Home Organisation CIC

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Move Home Organisation CIC (Companies House 17202438, incorporated 6 May 2026, Poole, England) is an asset-locked UK Community Interest Company that runs MoveHome.org, a free, not-for-profit property listing aggregator built as the open-source reference consumer of the RAIA Protocol — the Real Estate AI Agent standard whose charter names the same CIC as its governing body — with listings federated from estate agencies and the transaction side handled by the separate commercial company EstateAigents.com Ltd. Its machine surface is agent-native and anonymous: an A2A 0.3.0 agent card at /.well-known/agent-card.json (graded conformant) with search_properties, get_property and create_enquiry skills over JSON-RPC at https://movehome.org/api/a2a; a read-only MCP server at https://movehome.org/mcp and a second MCP server over its public registry of real-estate A2A agents; an unauthenticated registry REST API under /api/registry/v1; and, for CRMs pushing listings in, an OAuth2 client-credentials implementation of the RAIA Portal Feed API (OpenAPI 3.1.0, 13 operations, RFC 9457 problem details) at https://movehome.org/api/raia/portal/v1. The site publishes an llms.txt/skills.md for agents but no privacy policy, terms, status page, changelog, SDK or pricing page.

**Areas:** Real Estate · Property · Lettings · Property Sales · Agents · A2A · MCP · Agent-Native · Agent Registry · Non-Profit · Open Source · RAIA Protocol

## Move Home Organisation CIC on APIs.io

Everything profiled for Move Home Organisation CIC is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/movehome-org/](https://apis.io/providers/movehome-org/)** — or go straight to one of the APIs or properties below.

### APIs (7)

- [MoveHome.org A2A Property Agent](https://apis.io/apis/movehome-org/movehome-a2a-property-agent/) — Agent2Agent (A2A 0.3.0) surface: an agent card served from https://movehome.org/.well-known/agent-card.json (and the legacy /.well-known/agent.json,…
- [MoveHome.org Property MCP Server](https://apis.io/apis/movehome-org/movehome-property-mcp-server/) — Remote Model Context Protocol server at https://movehome.org/mcp (alias /api/mcp): stateless Streamable HTTP, protocol version 2025-06-18, serverInfo…
- [MoveHome.org A2A Agent Registry API](https://apis.io/apis/movehome-org/movehome-a2a-registry-api/) — Public registry of real-estate A2A agents operated by MoveHome, exposed two ways. REST under https://movehome.org/api/registry/v1 (no OpenAPI published;…
- [Move Home Organisation CIC Branches API](https://apis.io/apis/movehome-org/movehome-org-branches-api/) — Per-branch reconciliation, performance reporting and enquiry retrieval.
- [Move Home Organisation CIC Listings API](https://apis.io/apis/movehome-org/movehome-org-listings-api/) — Upload, update, retrieve and remove residential or commercial listings.
- [Move Home Organisation CIC Operational API](https://apis.io/apis/movehome-org/movehome-org-operational-api/) — Health and metadata.
- [Move Home Organisation CIC Products API](https://apis.io/apis/movehome-org/movehome-org-products-api/) — Portal product activations such as Premium Listings and Featured Properties.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Movehome Org Webhooks](https://apis.io/asyncapis/movehome-org/movehome-org-webhooks/)

#### MCP servers (2)

Model Context Protocol servers that expose the API to agents.

- [MoveHome A2A Registry MCP endpoint (Streamable HTTP)](https://apis.io/servers/movehome-org/movehome-a2a-registry-mcp-endpoint-streamable-http/)
- [movehome-org-mcp.yml](https://apis.io/servers/movehome-org/movehome-org-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Movehome Org Agentic Access](https://apis.io/agentic-access/movehome-org/movehome-org-agentic-access/)

#### JSON Schema (3)

The data models the API reads and writes.

- [RAIA Agent Card](https://apis.io/schemas/movehome-org/movehome-org-raia-agent-card/)
- [RAIA Enquiry](https://apis.io/schemas/movehome-org/movehome-org-raia-enquiry/)
- [RAIA Property Listing](https://apis.io/schemas/movehome-org/movehome-org-raia-listing/)

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Movehome Org Context](https://apis.io/jsonld/movehome-org/movehome-org-context/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Movehome Org Raia Portal Feed Overlay](https://apis.io/overlays/movehome-org/movehome-org-raia-portal-feed-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [Move Home Organisation CIC API Rules](https://apis.io/rules/movehome-org/movehome-org-rules/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Movehome Org Authentication](https://apis.io/security/movehome-org/movehome-org-authentication/)
- [Movehome Org Domain Security](https://apis.io/security/movehome-org/movehome-org-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Movehome Org Scopes](https://apis.io/scopes/movehome-org/movehome-org-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Movehome Org Plans Pricing](https://apis.io/plans/movehome-org/movehome-org-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Movehome Org Rate Limits](https://apis.io/rate-limits/movehome-org/movehome-org-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://movehome.org/)
- [Documentation](https://movehome.org/skills.md)
- [Getting started](https://movehome.org/skills.md)
- [API reference](https://github.com/MoveHome/MoveHome.Org/tree/main/docs)
- [Support](https://github.com/MoveHome/MoveHome.Org/issues)
- [GitHub](https://github.com/MoveHome)

## API Evangelist rating

**Kin Score 54.1** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 66.6 |
| Contract governance | 35.6 |
| Operational transparency | 44.7 |
| Developer ergonomics | 61.9 |
| Access clarity | 21.1 |

**Agent readiness 52.3** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | negotiable (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | verified |
| Event Surface Described | yes |
| Agent Skills | yes |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | conformant |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 26.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/movehome-org/](https://apis.io/providers/movehome-org/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Move Home Organisation CIC: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/movehome-org/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
