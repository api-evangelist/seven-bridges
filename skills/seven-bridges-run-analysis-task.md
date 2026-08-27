---
name: seven-bridges-run-analysis-task
description: Run a bioinformatics analysis on the Seven Bridges Platform - find an app, create a draft task with inputs, execute it, poll to completion and collect the output files.
api: Seven Bridges Platform API
generated: '2026-08-27'
method: generated
source: openapi/seven-bridges-platform-openapi.json, https://docs.sevenbridges.com/docs/the-api
operations:
  - list-all-your-projects
  - list-all-apps-available-to-you
  - get-details-of-an-app
  - get-raw-cwl-for-an-app
  - create-a-new-task
  - perform-an-action-on-a-specific-task
  - get-details-of-a-task
  - get-task-execution-details
  - abort-a-task
  - list-files-primary-method
---

# Run an analysis task on the Seven Bridges Platform

Base URL `https://api.sbgenomics.com/v2` (AWS US) or `https://eu-api.sbgenomics.com/v2` (AWS EU).
Every request carries `X-SBG-Auth-Token: <token>` and `Content-Type: application/json`.

## 1. Pick the project

`GET /projects` (`list-all-your-projects`). Projects are addressed as `owner/short-name`, **not** by their
display name — the short name is lower-cased with spaces and underscores replaced by hyphens. Passing a
display name produces platform error `3012` (malformed project id, expecting `owner/project`).

Use `?fields=id,name` to keep the response small and `?limit=100` (the documented maximum) to reduce the
number of calls you spend against the rate limit.

## 2. Find the app

`GET /apps?project={owner}/{project}` (`list-all-apps-available-to-you`). Add `visibility=PUBLIC` to search
the public app gallery; any other value than `PUBLIC` or `PRIVATE` returns error `6012`.

`GET /apps/{project_owner}/{project}/{app_short_name}` (`get-details-of-an-app`) returns the app, and
`GET /apps/{project_owner}/{project}/{app_short_name}/raw` (`get-raw-cwl-for-an-app`) returns the raw CWL,
which is where the real input port names and types live. Read the CWL before building inputs — the task
create call will reject unknown or missing ports with error `7024` (task cannot be started due to validation
errors) or `7012` (missing inputs).

App IDs are `owner/project/app_name/revision`; a malformed one raises `6011`.

## 3. Create the task as a draft

`POST /tasks` (`create-a-new-task`) with the project, the app id and an `inputs` object keyed by CWL input
port. This creates a task in `DRAFT` state — **nothing runs and nothing is billed yet**. This two-phase
create-then-run is the closest thing this API has to a dry run; there is no `validate_only` parameter.

For a batch task, supply `batch_input` and `batch_by` **together** — supplying one without the other returns
error `7023`, and an empty `batch_input` returns `7018`. `batch_type` accepts only `criteria` or `item`
(`7020`).

## 4. Run it

`POST /tasks/{task_id}/actions/run` (`perform-an-action-on-a-specific-task`). This only works on a `DRAFT`
task — calling it on anything else returns `7007`. Task IDs are UUIDs; a non-UUID returns `7005`.

## 5. Poll to completion

`GET /tasks/{task_id}` (`get-details-of-a-task`). States are `DRAFT`, `CREATING`, `QUEUED`, `RUNNING`,
`COMPLETED`, `ABORTED`, `FAILED`.

Poll with backoff. The rate limit is **1000 requests per five minutes per token**, and the response carries
`X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset` (a Unix timestamp) — read
`X-RateLimit-Remaining` and slow down before you exhaust it. Exhaustion returns `429` with platform code
`1000`; there is no `Retry-After` header, so wait until `X-RateLimit-Reset`.

For a long run, `GET /tasks/{task_id}/execution_details` (`get-task-execution-details`) gives per-job
progress without re-fetching the whole task.

## 6. Stopping and re-running

- `POST /tasks/{task_id}/actions/abort` (`abort-a-task`) — only valid while the task is `RUNNING` (`7008`) or
  in `CREATING`/`RUNNING` (`7011`). **Seven Bridges publishes no window and no refund statement for compute
  already consumed before an abort** — assume the spend is not reversible.
- `POST /tasks/{task_id}/actions/clone` (`rerun-a-task`) — clones and reruns.
- `DELETE /tasks/{task_id}` (`delete-a-task`) — removes the task record.
- Editing is only allowed on `DRAFT` tasks (`7026`).

## 7. Collect the outputs

Task outputs are files in the project. `GET /files?project={owner}/{project}` (`list-files-primary-method`)
filters by `origin.task`, so you can list exactly the files a task produced, then
`GET /files/{file_id}/download_info` for a download URL.

## Failure handling

The API returns an HTTP status **and** a numeric platform code in the JSON body. Task codes are the `7xxx`
class. Do not retry on `400`-class validation codes (`7012`, `7018`-`7024`) — they are deterministic. Retry
with backoff on `7000` (`503`, task service unavailable) and on `429`/`1000`.

**There is no idempotency key on this API.** A retried `POST /tasks` creates a second task and a second
analysis run, which costs money. Record the returned task id before retrying anything, and check
`GET /tasks?project=...` for an already-created task before re-issuing a create.
