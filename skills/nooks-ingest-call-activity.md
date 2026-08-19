---
name: Ingest Nooks call and email activity
description: >-
  Pull finalized calls, dispositions and email engagement out of Nooks — by polling the REST API
  or by consuming the signed call.logged webhook — and attribute each one to a prospect, sequence
  and rep.
api: openapi/nooks-sequencing-openapi.yml
base_url: https://partner-api.nooks.in/v1
operations:
  - listCalls
  - getCall
  - listCallDispositions
  - getCallDisposition
  - listEmails
  - getEmail
  - listMailboxes
  - getMailbox
  - getEmailTemplate
  - getProspect
  - getUser
scopes:
  - calls:read
  - call-dispositions:read
  - emails:read
  - email-templates:read
  - mailboxes:read
  - prospects:read
  - users:read
generated: '2026-08-14'
method: generated
source: openapi/nooks-sequencing-openapi.yml
---

# Ingest Nooks call and email activity

Two ways in. Prefer the webhook for freshness; use the REST poll for backfill and reconciliation.

## Option A — consume the `call.logged` webhook (push)

`call.logged` fires once a call is **fully finalized** — status, disposition, recording,
transcript and notes all resolved. It covers every call in the workspace: inbound and outbound,
dialer calls, manually logged calls, and calls placed from a sequence.

Set up in **Integrations → Webhooks**. Nooks sends a verification ping on save and expects a 2xx.
The signing key is shown **once** — store it before closing the dialog.

**Verify every delivery before trusting it:**

1. Read the `x-webhook-signature` header. Format: `t=<unix-ms>,s=<base64-hmac-sha256>`.
2. Reject anything with a timestamp older than **5 minutes** (replay protection).
3. Compute `HMAC-SHA256(t + "." + raw_body, signing_key)`, base64-encode it, and compare to `s`
   with a **timing-safe** comparison (`crypto.timingSafeEqual` or equivalent).
4. Use the **literal, unparsed** request body. Re-serializing the JSON changes the bytes and the
   signature will not match.

**Acknowledge fast.** Respond 2xx within **15 seconds**. Failures retry with exponential backoff
and jitter, up to **8 attempts over 30 minutes**.

**Deduplicate on `callData.callId` — not `eventId`.** A redelivered call carries the same
`callId` but a new `eventId`, so deduping on `eventId` double-processes every retry. Payload
fields: `event`, `eventId`, `occurredAt`, `callData` (with nested user, prospect, account and
sequence attribution).

## Option B — poll the REST API (pull)

1. `listCalls` (`GET /calls`) with
   `?include=prospect,sequence,sequenceStep,callDisposition,owner` — one call instead of five.
   Page with `page[size]` (max 100) + `page[after]`. Read `time`, `direction`, `duration`,
   `from`, `to`, `recordingUrl`.
2. `listCallDispositions` (`GET /callDispositions`) once and cache it — dispositions are a small
   workspace-level lookup (`name`, `callOutcome`, `order`), not per-call data.
3. `getCall` (`GET /calls/{id}`) for a single record.
4. `listEmails` (`GET /emails`) with `?include=prospect,sequence,sequenceStep` for email
   engagement: `deliveredAt`, `openedAt`, `clickedAt`, `bouncedAt`, `repliedAt`, `openCount`,
   `clickCount`.
5. `listMailboxes` / `getMailbox` to map a `from` address to the connected mailbox and its user.
6. `getEmailTemplate` (`GET /emailTemplate/{id}`) to resolve the template behind a sequence step.

## Rules an agent must follow

- **This flow is read-only.** No `calls:write` or `emails:write` operation exists in the
  published REST contract even though both scopes are granted by the authorization server — do
  not attempt call or email writes against this API.
- **Expand, don't fan out.** `include` exists precisely so you don't burn per-endpoint quota on
  `_href` follow-ups. Reads by id are 600/min but list reads are 300/min — a naive N+1 walk will
  hit `429` long before an expanded list will.
- **Cache the lookups.** Call dispositions and users change rarely; re-fetching them per call
  wastes the budget.
- **Trust the webhook only after signature verification.** An unverified POST to your endpoint is
  an unauthenticated request from the internet.

## Errors

| Status | Code | Meaning |
|---|---|---|
| 400 | `BAD_REQUEST` | Bad parameters — commonly a stale `page[after]` cursor |
| 401 | `UNAUTHORIZED` | Missing/expired credential |
| 403 | `INSUFFICIENT_SCOPE` | Token lacks `calls:read` / `emails:read` |
| 404 | `NOT_FOUND` | Wrong id or wrong workspace |
| 429 | `RATE_LIMIT_EXCEEDED` | Wait `Retry-After` seconds |
| 500 | `INTERNAL_ERROR` | Retry with backoff; reads are safe to retry |
