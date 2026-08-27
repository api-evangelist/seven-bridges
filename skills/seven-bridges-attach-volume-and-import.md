---
name: seven-bridges-attach-volume-and-import
description: Bring existing cloud-bucket data onto the Seven Bridges Platform - attach an S3/GCS/Azure volume, list its contents, bulk-import files into a project, and export results back out.
api: Seven Bridges Platform API
generated: '2026-08-27'
method: generated
source: openapi/seven-bridges-platform-openapi.json, https://docs.sevenbridges.com/reference/api-status-codes
operations:
  - create-a-volume-v2
  - list-volumes-v2
  - get-details-of-a-volume-v2
  - list-the-contents-of-a-volume
  - get-details-of-a-file-within-a-volume
  - start-an-import-job-v2
  - list-import-jobs
  - get-details-of-an-import-job-v2
  - start-an-export-job-v2
  - get-details-of-an-export-job-v2
  - add-members-to-a-volume
  - remove-members-from-a-volume
  - deactivate-a-volume-1
  - delete-a-volume-v2
---

# Attach cloud storage and move data in and out

Base URL `https://api.sbgenomics.com/v2`. Header `X-SBG-Auth-Token: <token>`.

A **volume** is a customer-owned cloud bucket (AWS S3, Google Cloud Storage, Azure, OSS) registered with the
Platform. Importing copies objects from the volume into a project as Platform files; exporting writes
Platform files back out.

## 1. Attach the volume

`POST /storage/volumes` (`create-a-volume-v2`). The body needs a `name`, an `access_mode` and a `service`
object describing the bucket and its credentials.

The published error registry is the real specification for this call:

- `9013` — volume name must not be empty.
- `9014` / `9030` — the name must be at most 32 characters, letters, numbers and underscores.
- `9015` — `access_mode` is required and must be `RO` or `RW`.
- `9016` / `9017` / `9019` — the `service` object is required and must be well-formed.
- `9010` — a volume with that name already exists.
- `9022`-`9025` — invalid `aws_canned_acl`, `sse_algorithm`, `aws_storage_class` or `private_key`.
- `9029` / `9031` / `9034` — unrecognized Google, Azure or S3 credential type.

Credential problems surface as their own class once the Platform tries the bucket: `9200` (bucket does not
exist), `9201`/`9202` (bad access or secret key), `9203` (IAM role not valid or misconfigured), and
`9204`-`9207` for a role missing LIST, GET, PUT or GET-ACL permission. Read these literally — they tell you
exactly which bucket policy statement is missing.

Some environments restrict what can be attached: `9038`, `9039`, `9057`, `9058` all mean this deployment only
supports read-only buckets of a particular type, and `9209` means an RW volume is not allowed in that bucket's
region.

## 2. Browse it before you import

`GET /storage/volumes/{volume_owner}/{volume_name}/list` (`list-the-contents-of-a-volume`) and
`GET /storage/volumes/{volume_owner}/{volume_name}/{object_id}` (`get-details-of-a-file-within-a-volume`).
A path ending in a slash is rejected with `9072`.

## 3. Import into a project

`POST /storage/imports` (`start-an-import-job-v2`). Per the docs this is a bulk call: **up to 100 items per
call**, and it can preserve folder structure when importing a folder into a destination folder.

Track it with `GET /storage/imports/{import_id}` (`get-details-of-an-import-job-v2`) — the one operation in
this contract that declares a `404` response — and `GET /storage/imports` to list jobs.

Import-side errors live in the `10xxx` class: `10222` (destination folder missing), `10223`/`10224` (file or
folder already exists), `10225`/`10226` (source missing or not permitted), `10227` (destination must be a
folder), `10246` (invalid import URI), `10248` (invalid metadata), and `10371` (`429`, rate limit exceeded on
the import service — distinct from the global `1000`).

If you are importing GA4GH DRS references, `10276` and `10278` mean the DRS bundle or blob was malformed.

## 4. Export results back out

`POST /storage/exports` (`start-an-export-job-v2`), tracked with
`GET /storage/exports/{export_id}` (`get-details-of-an-export-job-v2`, which declares a `409`).

Export needs an `RW` volume: `9026` and `9101` both mean the volume's `access_mode` is not `RW`. `9006` means
the file cannot be exported, and `9027` means cross-cloud export is not supported — you cannot export from a
GCS-backed project to an S3 volume.

## 5. Reversal — know what you can take back

- Volume access: `DELETE /storage/volumes/{o}/{n}/members/{username}` (`remove-members-from-a-volume`) undoes
  `add-members-to-a-volume`.
- The volume itself: `PATCH .../{volume_name}` (`deactivate-a-volume-1`) deactivates it and
  `DELETE .../{volume_name}` (`delete-a-volume-v2`) removes it. An inactive volume refuses downloads (`5034`)
  and other operations (`9069`).
- **Import and export jobs have no cancel or reverse operation** in the published contract — only `list` and
  `get-details`. Once a job is submitted, plan on it completing. No window is published for any of these.

## Failure handling

`9xxx` is volumes, `10xxx` is the import/manifest service. Retry `9000`/`9100` (`503`) and `10215`/`10220`.
Back off on `10371`. Everything in the `9200`-`9209` range is a bucket configuration problem on your side and
will fail identically on retry. There is no idempotency key, so re-issuing a `POST /storage/imports` after a
timeout can import the same objects twice — list existing jobs first.
