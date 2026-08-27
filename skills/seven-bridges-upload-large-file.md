---
name: seven-bridges-upload-large-file
description: Upload a large genomic file to a Seven Bridges project using the multipart upload API - initialize, request part URLs, report parts, finalize, and abort cleanly on failure.
api: Seven Bridges Platform API
generated: '2026-08-27'
method: generated
source: openapi/seven-bridges-platform-openapi.json, https://docs.sevenbridges.com/docs/the-api
operations:
  - initialize-a-multipart-upload
  - get-the-upload-url-for-a-file-part
  - report-an-uploaded-file-part
  - report-that-all-file-parts-have-uploaded
  - finalize-a-multipart-upload
  - abort-a-multipart-upload
  - list-current-multipart-uploads
  - get-details-of-a-multipart-upload
  - get-file-details
  - overwrite-a-files-metadata
---

# Upload a large file to a Seven Bridges project

Base URL `https://api.sbgenomics.com/v2`. Header `X-SBG-Auth-Token: <token>`.

Genomic files are large enough that the single-shot path is not an option; the Platform publishes a
multipart protocol where the API hands you a presigned URL per part and you PUT the bytes directly to
object storage.

## 1. Initialize

`POST /upload/multipart` (`initialize-a-multipart-upload`) with the destination project and the file name.
The response carries the `upload_id` you use for the rest of the flow. A malformed project id returns
platform error `8010`; an invalid init body returns `8013`. If the file already exists in the destination,
the API returns `8005`.

Record the `upload_id` immediately — it is the only handle you have to abort a half-finished upload.

## 2. Per part: get a URL, PUT the bytes, report it

For each part number:

1. `GET /upload/multipart/{upload_id}/part/{part_number}` (`get-the-upload-url-for-a-file-part`) — returns
   the URL to PUT this part to. A missing or invalid part number returns `8012`.
2. PUT the bytes to that URL directly. This request does **not** go through `api.sbgenomics.com` and does
   **not** consume your API rate limit.
3. `POST /upload/multipart/{upload_id}/part` (`report-an-uploaded-file-part`) with the part report. An
   invalid report returns `8014`; a failed part reservation returns `8008`, which is explicitly documented as
   retryable ("Try again").

Parts can be uploaded in parallel. Only steps 1 and 3 count against the 1000-requests-per-five-minutes limit,
so size your parts with that budget in mind: a file split into 2000 parts costs 4000 API calls and will hit
the limit twice.

You can also report every part in one call with `POST /upload/multipart/{upload_id}` — the body must be an
object with a `parts` array, or you get `8015`.

## 3. Finalize

`POST /upload/multipart/{upload_id}/complete` (`finalize-a-multipart-upload`). A failure here returns `8007`.
On success the upload becomes a File; fetch it with `GET /files/{file_id}` (`get-file-details`).

## 4. Attach metadata

Genomic files are only useful once they carry sample metadata.
`PUT /files/{file_id}/metadata` (`overwrite-a-files-metadata`) replaces the whole document;
`PATCH /files/{file_id}/metadata` merges. Metadata that fails validation returns `5006`. Note that folders
reject metadata updates entirely (`5025`).

## 5. Always have an abort path

`DELETE /upload/multipart/{upload_id}` (`abort-a-multipart-upload`) cancels the upload and releases the
parts. This is the reversal operation for step 1; **Seven Bridges publishes no time window on it**, so treat
an abandoned upload as something you must clean up yourself rather than something that expires on a known
schedule.

To find uploads you abandoned earlier, `GET /upload/multipart` (`list-current-multipart-uploads`), and
`GET /upload/multipart/{upload_id}` (`get-details-of-a-multipart-upload`) for one of them. Failing to abort
returns `8009`.

## Failure handling

`8xxx` is the upload error class. Retry `8008` (part reservation) and `8000` (`503`, upload service
unavailable). Do not retry `8013`/`8014`/`8015` — they are malformed requests. There is no idempotency key,
so a blind retry of `POST /upload/multipart` starts a *second* upload; check
`GET /upload/multipart` before re-initializing.
