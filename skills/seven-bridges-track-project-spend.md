---
name: seven-bridges-track-project-spend
description: Read Seven Bridges compute, storage and egress spend programmatically - list billing groups, pull analysis/storage/egress breakdowns, and reconcile against invoices.
api: Seven Bridges Platform API
generated: '2026-08-27'
method: generated
source: openapi/seven-bridges-platform-openapi.json, https://docs.sevenbridges.com/reference/the-api
operations:
  - list-your-billing-groups
  - get-a-single-billing-group
  - get-analysis-breakdown-for-a-billing-group
  - get-storage-breakdown-for-a-billing-group
  - get-egress-breakdown-for-a-billing-group
  - list-invoices
  - get-a-specific-invoice
  - get-your-current-rate-limit-status
---

# Track spend on the Seven Bridges Platform

Base URL `https://api.sbgenomics.com/v2`. Header `X-SBG-Auth-Token: <token>`.

Seven Bridges publishes **no public rate card** — pricing is quoted through sales — but the Platform is
metered and the meters are readable through the API. This is how you find out what an analysis actually cost.

## 1. Find the billing group

`GET /billing/groups` (`list-your-billing-groups`), then
`GET /billing/groups/{billing_group}` (`get-a-single-billing-group`).

Billing group IDs are UUIDs. A non-UUID returns platform error `4005`; an empty one returns `4004`. If you are
not a member of the group you get `4006`, and `4001` covers insufficient privileges generally. Every project
is created against a billing group — omitting it on project creation returns `3008`.

## 2. Pull the three meters

All three take `date_from` and `date_to` query parameters:

- `GET /billing/groups/{billing_group}/breakdown/analysis` (`get-analysis-breakdown-for-a-billing-group`) —
  compute spend, which is where task runs land.
- `GET /billing/groups/{billing_group}/breakdown/storage` (`get-storage-breakdown-for-a-billing-group`) —
  what the project's files cost to keep.
- `GET /billing/groups/{billing_group}/breakdown/egress` (`get-egress-breakdown-for-a-billing-group`) — what
  moving data out cost, which is the line people forget when they export results to their own bucket.

Use the `fields` query parameter to trim these responses; they can be large over a wide date range.

## 3. Reconcile against invoices

`GET /billing/invoices` (`list-invoices`) and `GET /billing/invoices/{invoice_id}`
(`get-a-specific-invoice`). An empty invoice id returns `4007`; a missing group or invoice returns `4002`.

## 4. Attribute cost back to work

The API does not join spend to tasks for you. The practical approach is to use the analysis breakdown for the
window a task ran in, and `GET /tasks/{task_id}/execution_details` (`get-task-execution-details`) for the jobs
and instances that task consumed.

Note also that a token has an **instance limit** as well as a request limit —
`GET /rate_limit` (`get-your-current-rate-limit-status`) returns both. The instance limit is the ceiling on
concurrent compute, and it is the number that determines how much you can spend at once.

## Cadence and rate limit

Billing breakdowns are reporting queries, not real-time telemetry. Pull them on a schedule rather than in a
loop: the whole API shares one budget of 1000 requests per five minutes per token, and a spend-monitoring job
that polls aggressively will starve the analysis and file calls that are doing the actual work. Read
`X-RateLimit-Remaining` and back off before `429`/code `1000`.

## Failure handling

`4xxx` is the billing class. `4000` is a `503` (billing service unavailable) and is worth retrying with
backoff; `4001`, `4002`, `4004`-`4007` are deterministic and will fail identically on retry. These are all
read operations, so there is no idempotency concern here.
