# Seven Bridges

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

Seven Bridges (a Velsera company) is a biomedical data analysis company whose cloud platform lets research organizations store, organize and analyze genomic and multi-omic data at scale. The Seven Bridges Platform exposes a public REST API at api.sbgenomics.com/v2 (AWS US) and eu-api.sbgenomics.com/v2 (AWS EU) covering projects, members and permissions, files and folders, file metadata and tags, multipart uploads, cloud storage volumes with import/export jobs, CWL and Nextflow apps and their revisions, task creation/execution/batching, billing groups, invoices and cost breakdowns, plus rate limit and advanced file search. The same API powers two NIH/NCI-funded deployments Seven Bridges operates - the Cancer Genomics Cloud (cgc-api.sbgenomics.com) and CAVATICA (cavatica-api.sbgenomics.com). Authentication is a per-user X-SBG-Auth-Token header, first-party Python and R client libraries and an `sb` command line interface are published, and the API is documented on a ReadMe developer hub that also serves an llms.txt index.

**Areas:** Genomics · Bioinformatics · Life Sciences · Healthcare · Cloud Computing · Data Analysis · Workflow Automation · Research · Precision Medicine · Common Workflow Language · File Storage · Company

## Seven Bridges on APIs.io

Everything profiled for Seven Bridges is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/seven-bridges/](https://apis.io/providers/seven-bridges/)** — or go straight to one of the APIs or properties below.

### APIs (17)

- [Seven Bridges CAVATICA API](https://apis.io/apis/seven-bridges/seven-bridges-cavatica-api/) — CAVATICA is a pediatric-research data analysis platform Seven Bridges operates with the Children's Hospital of Philadelphia and the NIH Common Fund. It…
- [Seven Bridges Action API](https://apis.io/apis/seven-bridges/seven-bridges-action-api/) — The Action API from Seven Bridges — 3 operation(s) for action.
- [Seven Bridges API Settings API](https://apis.io/apis/seven-bridges/seven-bridges-api-settings-api/) — The API Settings API from Seven Bridges — 1 operation(s) for api settings.
- [Seven Bridges Apps API](https://apis.io/apis/seven-bridges/seven-bridges-apps-api/) — The Apps API from Seven Bridges — 8 operation(s) for apps.
- [Seven Bridges Billing API](https://apis.io/apis/seven-bridges/seven-bridges-billing-api/) — The Billing API from Seven Bridges — 8 operation(s) for billing.
- [Seven Bridges Bulk API](https://apis.io/apis/seven-bridges/seven-bridges-bulk-api/) — The Bulk API from Seven Bridges — 1 operation(s) for bulk.
- [Seven Bridges Files API](https://apis.io/apis/seven-bridges/seven-bridges-files-api/) — The Files API from Seven Bridges — 10 operation(s) for files.
- [Seven Bridges Https: API](https://apis.io/apis/seven-bridges/seven-bridges-https-api/) — The Https: API from Seven Bridges — 1 operation(s) for https:.
- [Seven Bridges Projects API](https://apis.io/apis/seven-bridges/seven-bridges-projects-api/) — The Projects API from Seven Bridges — 9 operation(s) for projects.
- [Seven Bridges Rate Limit API](https://apis.io/apis/seven-bridges/seven-bridges-rate-limit-api/) — The Rate Limit API from Seven Bridges — 1 operation(s) for rate limit.
- [Seven Bridges Search API](https://apis.io/apis/seven-bridges/seven-bridges-search-api/) — The Search API from Seven Bridges — 1 operation(s) for search.
- [Seven Bridges Seven Bridges Platform API](https://apis.io/apis/seven-bridges/seven-bridges-seven-bridges-platform-api/) — The Seven Bridges Platform API from Seven Bridges — 1 operation(s) for seven bridges platform.
- [Seven Bridges Storage API](https://apis.io/apis/seven-bridges/seven-bridges-storage-api/) — The Storage API from Seven Bridges — 14 operation(s) for storage.
- [Seven Bridges Tasks API](https://apis.io/apis/seven-bridges/seven-bridges-tasks-api/) — The Tasks API from Seven Bridges — 8 operation(s) for tasks.
- [Seven Bridges Upload API](https://apis.io/apis/seven-bridges/seven-bridges-upload-api/) — The Upload API from Seven Bridges — 8 operation(s) for upload.
- [Seven Bridges User API](https://apis.io/apis/seven-bridges/seven-bridges-user-api/) — The User API from Seven Bridges — 1 operation(s) for user.
- [Seven Bridges Users API](https://apis.io/apis/seven-bridges/seven-bridges-users-api/) — The Users API from Seven Bridges — 1 operation(s) for users.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [seven-bridges-mcp.yml](https://apis.io/servers/seven-bridges/seven-bridges-mcp-yml/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Seven Bridges Platform Overlay](https://apis.io/overlays/seven-bridges/seven-bridges-platform-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Seven Bridges Authentication](https://apis.io/security/seven-bridges/seven-bridges-authentication/)
- [Seven Bridges Domain Security](https://apis.io/security/seven-bridges/seven-bridges-domain-security/)
- [Seven Bridges Trust Center](https://apis.io/security/seven-bridges/seven-bridges-trust-center/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Seven Bridges Plans Pricing](https://apis.io/plans/seven-bridges/seven-bridges-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Seven Bridges Rate Limits](https://apis.io/rate-limits/seven-bridges/seven-bridges-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.sevenbridges.com/)
- [Documentation](https://docs.sevenbridges.com/)
- [Getting started](https://docs.sevenbridges.com/docs/quickstart)
- [API reference](https://docs.sevenbridges.com/reference/the-api)
- [Support](https://www.sevenbridges.com/contact/)
- [Blog](https://www.sevenbridges.com/blog/)
- [Terms of service](https://www.sevenbridges.com/terms-of-service/)
- [Privacy policy](https://www.sevenbridges.com/privacy-policy/)
- [GitHub](https://github.com/sbg)

## API Evangelist rating

**Kin Score 47.0** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 47.5 |
| Contract governance | 18.2 |
| Operational transparency | 23.7 |
| Developer ergonomics | 58.9 |
| Access clarity | 43.4 |

**Agent readiness 28.7** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | paginated |
| Operation Distinctiveness | — |

**Regulatory alignment 23.4** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/seven-bridges/](https://apis.io/providers/seven-bridges/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Seven Bridges: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/seven-bridges/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
