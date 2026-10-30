# Nooks

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

Nooks is an AI-native revenue/sales engagement platform positioned as "The Agent Workspace for Intelligent Outbound," where sales representatives and AI agents collaborate on prospecting, sequencing, dialing, and coaching inside a single workspace. Its products include an AI Parallel Dialer, AI Sequencing, AI Coaching, Signals Intelligence, and a Virtual Salesfloor, aimed at helping revenue and SDR teams build more pipeline with less manual work. Nooks publishes a public Nooks Sequencing API (the "Nooks External API") documented with a Scalar-rendered OpenAPI 3.1.0 contract at developer.nooks.in, served from partner-api.nooks.in/v1, covering sequences, sequence steps, sequence states, prospects, accounts, notes, tasks, calls, call dispositions, emails, email templates, mailboxes, and users. Authentication accepts either a workspace-scoped `nooks-api-` API key or an OAuth 2.0 authorization-code + PKCE access token from oauth.nooks.in, with 24 published scopes, RFC 8414 authorization-server metadata, and a JWKS endpoint. Nooks also operates an OAuth-protected remote MCP server at mcp.nooks.in, a signed `call.logged` webhook, an Atlassian Statuspage at status.nooks.in, and a SafeBase trust center with SOC 2 Type 2, SOC 3, ISO/IEC 27001:2022, GDPR, CCPA, and EU-US DPF coverage plus a Bugcrowd responsible-disclosure program. Nooks is backed by Kleiner Perkins and runs its application at app.nooks.in.

**Areas:** Company · Artificial Intelligence · Sales Engagement · Sales Dialer · AI SDR · Outbound Sales · Sales Coaching · Revenue Operations · Sales Sequencing · CRM Integration · Agents · MCP

## Nooks on APIs.io

Everything profiled for Nooks is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/nooks/](https://apis.io/providers/nooks/)** — or go straight to one of the APIs or properties below.

### APIs (16)

- [Nooks MCP Server](https://apis.io/apis/nooks/nooks-mcp-server/) — Nooks operates a hosted, remote Model Context Protocol server at https://mcp.nooks.in/. It is OAuth-protected — an anonymous `tools/list` returns HTTP 401…
- [Nooks Accounts API](https://apis.io/apis/nooks/nooks-accounts-api/) — Access account (company) records
- [Nooks Calls API](https://apis.io/apis/nooks/nooks-calls-api/) — Access call records
- [Nooks Emails API](https://apis.io/apis/nooks/nooks-emails-api/) — Access email records
- [Nooks Introspection API](https://apis.io/apis/nooks/nooks-introspection-api/) — Inspect the authenticated principal
- [Nooks Mailboxes API](https://apis.io/apis/nooks/nooks-mailboxes-api/) — Manage mailboxes (email aliases)
- [Nooks Sequencing API](https://apis.io/apis/nooks/nooks-nooks-sequencing-api-api/) — The Nooks Sequencing API API from Nooks — 0 operation(s) for nooks sequencing api.
- [Nooks Notes API](https://apis.io/apis/nooks/nooks-notes-api/) — Create CRM notes on prospects and accounts
- [Nooks Prospects API](https://apis.io/apis/nooks/nooks-prospects-api/) — Manage prospects
- [Nooks Sequences API](https://apis.io/apis/nooks/nooks-sequences-api/) — Manage sales sequences
- [Nooks Sequence Steps API](https://apis.io/apis/nooks/nooks-sequencesteps-api/) — Access sequence step definitions
- [Nooks Tasks API](https://apis.io/apis/nooks/nooks-tasks-api/) — Manage tasks (one-off calls and email activities)
- [Nooks Users API](https://apis.io/apis/nooks/nooks-users-api/) — Manage workspace users
- [Nooks Call Dispositions API](https://apis.io/apis/nooks/nooks-call-dispositions-api/) — Access call disposition definitions
- [Nooks Email Templates API](https://apis.io/apis/nooks/nooks-email-templates-api/) — Access email template content
- [Nooks Sequence States API](https://apis.io/apis/nooks/nooks-sequence-states-api/) — Track prospect enrollments in sequences

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Nooks Webhooks](https://apis.io/asyncapis/nooks/nooks-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Nooks MCP Server](https://apis.io/servers/nooks/nooks-mcp-server/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Nooks Agentic Access](https://apis.io/agentic-access/nooks/nooks-agentic-access/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Nooks Sequencing Overlay](https://apis.io/overlays/nooks/nooks-sequencing-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Nooks Authentication](https://apis.io/security/nooks/nooks-authentication/)
- [Nooks Domain Security](https://apis.io/security/nooks/nooks-domain-security/)
- [Nooks Trust Center](https://apis.io/security/nooks/nooks-trust-center/)
- [Nooks Vulnerability Disclosure](https://apis.io/security/nooks/nooks-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Nooks Scopes](https://apis.io/scopes/nooks/nooks-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Nooks Plans Pricing](https://apis.io/plans/nooks/nooks-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Nooks Rate Limits](https://apis.io/rate-limits/nooks/nooks-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.nooks.ai)
- [Documentation](https://developer.nooks.in/)
- [API reference](https://developer.nooks.in/)
- [Pricing](https://www.nooks.ai/pricing)
- [Support](https://nooks.help.usepylon.com/)
- [Blog](https://www.nooks.ai/blog)
- [Security](https://www.nooks.ai/responsible-disclosure-process)
- [Compliance](https://trust.nooks.ai/)
- [Terms of service](https://www.nooks.ai/terms-of-services)
- [Privacy policy](https://www.nooks.ai/privacy-policy)
- [GitHub](https://github.com/NooksApp)
- [Status](https://status.nooks.in/)

## API Evangelist rating

**Kin Score 60.4** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 64.2 |
| Contract governance | 4.5 |
| Operational transparency | 68.4 |
| Developer ergonomics | 47.0 |
| Access clarity | 53.9 |

**Agent readiness 58.7** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | paginated |
| Operation Distinctiveness | distinct |

**Regulatory alignment 41.8** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/nooks/](https://apis.io/providers/nooks/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Nooks: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/nooks/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
