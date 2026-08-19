---
name: Work a rep's Nooks task queue
description: >-
  Read a rep's outstanding Nooks tasks, complete or skip them, and write the outcome back to the
  CRM as a note.
api: openapi/nooks-sequencing-openapi.yml
base_url: https://partner-api.nooks.in/v1
operations:
  - getMe
  - listUsers
  - listTasks
  - getTask
  - createTask
  - updateTask
  - completeTask
  - skipTask
  - deleteTask
  - createProspectNote
  - createAccountNote
scopes:
  - users:read
  - tasks:read
  - tasks:write
  - prospects:read
  - accounts:read
  - notes:write
generated: '2026-08-14'
method: generated
source: openapi/nooks-sequencing-openapi.yml
---

# Work a rep's Nooks task queue

## Authenticate

`Authorization: Bearer <token>`. Start with `getMe` (`GET /me`) to resolve the caller's identity
and workspace — with an OAuth token this tells you *which rep* you are acting as, which matters
for every task write below.

## Steps

1. **Identify the rep.** `getMe`, or `listUsers` (`GET /users`) when acting on someone else's
   queue. You need a real workspace user id: setting `owner.id` / `ownerId` to anything else
   returns **422**, and the error tells you to use an id from `GET /v1/users`.

2. **Read the queue.** `listTasks` (`GET /tasks`). Add
   `?include=prospect,owner,sequence,sequenceState,sequenceStep` so you get the prospect and
   sequence context inline. Page with `page[size]` (max 100) + `page[after]`. Read `status`,
   `priority`, `dueAt`, `action` and `completed` to decide order of work.

3. **Inspect one task.** `getTask` (`GET /tasks/{id}`) for the full record.

4. **Act on it.**
   - `completeTask` (`POST /tasks/{id}/complete`) — the task was done.
   - `skipTask` (`POST /tasks/{id}/skip`) — the task should not be done.
   - `updateTask` (`PATCH /tasks/{id}`) — change `dueAt`, `note`, `priority`, `status` or owner.
   - `createTask` (`POST /tasks`) — add follow-up work.
   - `deleteTask` (`DELETE /tasks/{id}`) — remove it entirely; returns `204`.

5. **Write the outcome back to the CRM.** `createProspectNote`
   (`POST /prospects/{id}/notes`) or `createAccountNote` (`POST /accounts/{id}/notes`). Set
   `integrationType` to the CRM the record actually came from — a mismatch is a **422**. These
   endpoints are limited to **30 requests/minute each**, the second-tightest bucket in the API.

## Rules an agent must follow

- **Writes are not idempotent.** There is no `Idempotency-Key` header anywhere in this API. If
  `createTask` or `createProspectNote` times out, re-issuing it can create a duplicate. Recover
  by calling `listTasks` and checking whether the task exists rather than retrying the POST.
  `completeTask` and `skipTask` are safer because they converge on a terminal state, but neither
  is documented as idempotent — verify with `getTask` instead of retrying blind.
- **Check `error.details.message` on 422.** Documented causes include: `ownerId` is not a user in
  this workspace; task creation prerequisites not met (e.g. the prospect is marked Do Not Call);
  the update violates task business rules; `integrationType` does not match the record's CRM
  source; the CRM record is not writable.
- **`NO_CRM_CONNECTION` (400) is a setup problem, not a retry.** The acting user's CRM connection
  is missing or unusable — surface it to a human rather than looping.
- **Budget the note writes.** 30/min per endpoint. Task writes get 120/min. List reads 300/min,
  reads by id 600/min. All per endpoint, per minute, and all reported on
  `X-RateLimit-Remaining`.
- **Never assume a Do Not Call prospect can be worked.** That is an explicit 422 cause; treat it
  as terminal for that prospect.

## Errors

| Status | Code | Meaning |
|---|---|---|
| 400 | `BAD_REQUEST` | Bad parameters or a stale cursor |
| 400 | `NO_CRM_CONNECTION` | Acting user has no usable CRM connection |
| 401 | `UNAUTHORIZED` | Missing/expired credential |
| 403 | `INSUFFICIENT_SCOPE` | Token lacks `tasks:write` or `notes:write` |
| 404 | `NOT_FOUND` | Wrong id or wrong workspace |
| 422 | `UNPROCESSABLE_ENTITY` | Business rule blocked the write — read `error.details.message` |
| 429 | `RATE_LIMIT_EXCEEDED` | Wait `Retry-After` seconds |
| 500 | `INTERNAL_ERROR` | Back off; verify before retrying a write |
