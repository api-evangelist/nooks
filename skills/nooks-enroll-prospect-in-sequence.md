---
name: Enroll a prospect in a Nooks sequence
description: >-
  Find or sync a prospect, pick the right sequence, enroll the prospect, and confirm the
  enrollment landed — the core Nooks Sequencing API flow.
api: openapi/nooks-sequencing-openapi.yml
base_url: https://partner-api.nooks.in/v1
operations:
  - listSequences
  - getSequence
  - listProspects
  - getProspect
  - syncProspects
  - createSequenceState
  - getSequenceState
  - listSequenceStates
scopes:
  - sequences:read
  - prospects:read
  - prospects:write
  - sequence-states:read
  - sequence-states:write
generated: '2026-08-14'
method: generated
source: openapi/nooks-sequencing-openapi.yml
---

# Enroll a prospect in a Nooks sequence

## Authenticate

Send `Authorization: Bearer <token>` on every request. The token is either a workspace API key
(prefixed `nooks-api-`, from Developer Settings → API Keys) or a 1-hour OAuth 2.0 access token
from `https://oauth.nooks.in`. Call `getMe` (`GET /me`) first to confirm which workspace the
credential resolves to before writing anything.

## Steps

1. **Pick the sequence.** `listSequences` (`GET /sequences`). Page with `page[size]` (max 100)
   and `page[after]`. Filter client-side on `enabled: true` — enrolling into a disabled sequence
   is not useful. Use `getSequence` if you already hold an id and need the step list.

2. **Resolve the prospect.** `listProspects` (`GET /prospects`) to find an existing record, or
   `getProspect` (`GET /prospects/{id}`) by id. Add `?include=account,sequenceStates` so you get
   the account and current enrollments inline instead of following `_href` stubs.

3. **Sync the prospect if it is not in Nooks yet.** `syncProspects`
   (`POST /integrations/prospects/sync`) pulls prospects in from the connected CRM. This endpoint
   is capped at **10 requests/minute** — the tightest limit in the API. It returns `200` for the
   batch and reports per-record outcomes in `results` and `errors`; a 200 does **not** mean every
   record succeeded, so always inspect both arrays.

4. **Check for an existing enrollment before writing.** `listSequenceStates`
   (`GET /sequenceStates`), or read the `sequenceStates` you included in step 2. Enrolling a
   prospect who is already in the sequence returns **409 Conflict**.

5. **Enroll.** `createSequenceState` (`POST /sequenceStates`) with the sequence and prospect ids.
   Returns **201** with the new `SequenceState`.

6. **Confirm.** `getSequenceState` (`GET /sequenceStates/{id}`) and check `state`.

## Stopping an enrollment

- `finishSequenceState` (`POST /sequenceStates/{id}/actions/finish`) marks the enrollment
  finished. **This one is idempotent** — it returns `204` even if the state was already finished,
  so it is safe to retry.
- `deleteSequenceState` (`DELETE /sequenceStates/{id}`) removes the prospect from the sequence
  entirely. Returns `204`.

## Rules an agent must follow

- **Do not blindly retry a failed write.** Nooks publishes no `Idempotency-Key` header. If
  `createSequenceState` times out, do **not** re-POST — call `listSequenceStates` and check
  whether the enrollment exists first. `finishSequenceState` is the only write that is safe to
  retry unconditionally.
- **Respect the rate limits.** Every response carries `X-RateLimit-Limit`,
  `X-RateLimit-Remaining` and `X-RateLimit-Reset`. On `429`, sleep for `Retry-After` seconds. The
  relevant buckets here: list reads 300/min, reads by id 600/min, sequence-state writes 120/min,
  prospect sync 10/min — all per endpoint, per minute.
- **Handle 422 as a business-rule failure, not a bad request.** Enrollment prerequisites include
  the prospect having a valid email. The `error.details.message` names the actual cause.
- **A 429 may be the CRM, not Nooks.** Salesforce/HubSpot limits hit while servicing the request
  surface as a Nooks 429. Back off the same way.
- **Capture `traceId`** from any error body and include it in support requests.

## Errors

| Status | Code | Meaning |
|---|---|---|
| 400 | `BAD_REQUEST` | Bad parameters — commonly a stale `page[after]` cursor |
| 400 | `NO_CRM_CONNECTION` | The CRM connection is missing or unusable for the acting user |
| 401 | `UNAUTHORIZED` | Missing/expired credential — OAuth tokens live 1 hour |
| 403 | `INSUFFICIENT_SCOPE` | Token lacks `sequence-states:write` (or the caller's role forbids it) |
| 404 | `NOT_FOUND` | Wrong id, or the record is in another workspace |
| 409 | — | Prospect is already enrolled in this sequence |
| 422 | `UNPROCESSABLE_ENTITY` | Prerequisites not met — read `error.details.message` |
| 429 | `RATE_LIMIT_EXCEEDED` | Wait `Retry-After` seconds |
| 500 | `INTERNAL_ERROR` | Retry with backoff, but see the idempotency rule above |
