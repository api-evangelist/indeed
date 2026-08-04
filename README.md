# Indeed (indeed)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Indeed is the world's largest job site, connecting millions of job seekers with employers across industries and locations worldwide. Indeed offers a suite of APIs for applicant tracking systems, job boards, and hiring platforms to integrate with its employment ecosystem.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/indeed/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/indeed/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Access:** 3rd-Party

## Tags

- Careers
- Employment
- Hiring
- Job Search
- Jobs
- Recruiting

## Timestamps

- **Created:** 2025-01-01
- **Modified:** 2026-05-19

## APIs

### Indeed Job Search API

Search for jobs by keyword, location, and other criteria. Returns job listings with details including title, company, location, and description. This API is deprecated and not available for new integrations.

- **Human URL:** [https://developer.indeed.com/docs/publisher-jobs/job-search](https://developer.indeed.com/docs/publisher-jobs/job-search)
- **Base URL:** `https://api.indeed.com`

#### Tags

- Deprecated
- Jobs
- Listings
- Search

#### Properties

- [Documentation](https://opensource.indeedeng.io/api-documentation/docs/job-search/)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Publisher API

Monetize your website by displaying Indeed job listings. Earn revenue through cost-per-click advertising.

- **Human URL:** [https://www.indeed.com/publisher](https://www.indeed.com/publisher)
- **Base URL:** `https://api.indeed.com/ads`

#### Tags

- Advertising
- Monetization
- Publisher

#### Properties

- [Documentation](https://www.indeed.com/publisher/docs)
- [Sign Up](https://www.indeed.com/publisher/signup)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Apply API

Allow job seekers to apply to your jobs directly through Indeed with a streamlined application process. Supports screener questions, EEO compliance for US employers, and disposition data integration.

- **Human URL:** [https://docs.indeed.com/indeed-apply](https://docs.indeed.com/indeed-apply)
- **Base URL:** `https://api.indeed.com/apply`

#### Tags

- Applications
- Apply
- Hiring

#### Properties

- [Documentation](https://docs.indeed.com/indeed-apply)
- [Getting Started](https://docs.indeed.com/indeed-apply/add-indeed-apply)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Job Sync API

A GraphQL API that enables ATS partners to create, upsert, expire, and get status for job postings on Indeed. Supports qualifications, working hours, salary, benefits, and employer information.

- **Human URL:** [https://docs.indeed.com/job-sync-api/](https://docs.indeed.com/job-sync-api/)
- **Base URL:** `https://apis.indeed.com/graphql`

#### Tags

- ATS
- GraphQL
- Jobs
- Postings

#### Properties

- [Documentation](https://docs.indeed.com/job-sync-api/)
- [Getting Started](https://docs.indeed.com/job-sync-api/job-sync-api-guide)
- [F A Q](https://docs.indeed.com/job-sync-api/reference/faq)
- [Sandbox](https://docs.indeed.com/getstarted/simulated-graphql-environment)
- [OpenAPI](openapi/indeed-employer-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [JSON-LD](json-ld/indeed-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### Indeed Disposition Sync API

A GraphQL API that enables ATS partners to send disposition data for Indeed Apply and non-Indeed Apply jobs to Indeed, tracking application status changes through various stages of the hiring process.

- **Human URL:** [https://docs.indeed.com/disposition-sync-api/](https://docs.indeed.com/disposition-sync-api/)
- **Base URL:** `https://apis.indeed.com/graphql`

#### Tags

- Applications
- ATS
- Disposition
- GraphQL
- Tracking

#### Properties

- [Documentation](https://docs.indeed.com/disposition-sync-api/)
- [Getting Started](https://docs.indeed.com/disposition-sync-api/disposition-sync-api-guide)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Sponsored Jobs API

A GraphQL API used to get information about and manage an employer's sponsored job campaigns on Indeed, including campaign creation, budget management, and performance insights.

- **Human URL:** [https://docs.indeed.com/sponsored-jobs-api/](https://docs.indeed.com/sponsored-jobs-api/)
- **Base URL:** `https://apis.indeed.com/graphql`

#### Tags

- Advertising
- Campaigns
- GraphQL
- Sponsored

#### Properties

- [Documentation](https://docs.indeed.com/sponsored-jobs-api/)
- [Getting Started](https://docs.indeed.com/sponsored-jobs-api/sponsored-jobs-api-1-guides/get-started)
- [API Reference](https://docs.indeed.com/api/sponsored-jobs-api/sponsored-jobs-api-reference)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Job Update API

Allows partners to list and update job postings on Indeed, including adding metadata to ATS-sourced jobs for improved quality and sponsorship grouping, and subscribing to jobs lifecycle events via webhooks.

- **Human URL:** [https://docs.indeed.com/job-update-api/](https://docs.indeed.com/job-update-api/)
- **Base URL:** `https://apis.indeed.com/graphql`

#### Tags

- GraphQL
- Jobs
- Updates
- Webhooks

#### Properties

- [Documentation](https://docs.indeed.com/job-update-api/)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Real-time API

Stream real-time server-sent events (SSE) to enable front-end applications to update instantly, supporting event filtering, deduplication, and latency tracking.

- **Human URL:** [https://docs.indeed.com/real-time-api/get-started](https://docs.indeed.com/real-time-api/get-started)
- **Base URL:** `https://apis.indeed.com`

#### Tags

- Events
- Real-Time
- SSE
- Streaming

#### Properties

- [Documentation](https://docs.indeed.com/api/real-time-api/indeed-real-time-api)
- [Getting Started](https://docs.indeed.com/real-time-api/get-started)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Interview API

A GraphQL API for scheduling, updating, retrieving information about, and canceling virtual interview events with job candidates. This API is deprecated.

- **Human URL:** [https://docs.indeed.com/indeed-interview-api](https://docs.indeed.com/indeed-interview-api)
- **Base URL:** `https://apis.indeed.com/graphql`

#### Tags

- Deprecated
- GraphQL
- Interviews
- Scheduling
- Virtual

#### Properties

- [Documentation](https://docs.indeed.com/indeed-interview-api)
- [Getting Started](https://docs.indeed.com/indeed-interview-api/indeed-interview-api-guide)
- [API Reference](https://docs.indeed.com/dev/reference/indeed-interview-api)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Employer API

A RESTful abstraction of Indeed's employer-facing partner APIs, providing unified access to employer management, candidate retrieval, and job posting operations.

- **Human URL:** [https://docs.indeed.com/employers/operations/create-employer](https://docs.indeed.com/employers/operations/create-employer)
- **Base URL:** `https://apis.indeed.com`

#### Tags

- ATS
- Candidates
- Employers
- Jobs

#### Properties

- [Documentation](https://docs.indeed.com/employers/operations/create-employer)
- [OpenAPI](openapi/indeed-employer-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [JSON Schema](json-schema/indeed-candidate-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/indeed-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)

### Indeed Employer Data API

A GraphQL API for creating and updating employer entities on Indeed and the Indeed PLUS platform.

- **Human URL:** [https://docs.indeed.com/employers/operations/create-employer](https://docs.indeed.com/employers/operations/create-employer)
- **Base URL:** `https://apis.indeed.com/graphql`

#### Tags

- ATS
- Employers
- GraphQL

#### Properties

- [Documentation](https://docs.indeed.com/employers/operations/create-employer)
- [API Reference](https://docs.indeed.com/api/employers-api/create-using-post)
- [Sandbox](https://docs.indeed.com/getstarted/simulated-graphql-environment)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Conversion Tracking API

Tracks candidate events such as job application page visits and completed applications from Indeed to your site. Provides data for reporting, analytics dashboards, and apply-based bidding algorithms.

- **Human URL:** [https://docs.indeed.com/conversion-tracking-api/conversion-tracking-getting-started](https://docs.indeed.com/conversion-tracking-api/conversion-tracking-getting-started)
- **Base URL:** `https://apis.indeed.com`

#### Tags

- Analytics
- Conversion
- Reporting
- Tracking

#### Properties

- [Documentation](https://docs.indeed.com/conversion-tracking-api/conversion-tracking-getting-started)
- [API Reference](https://docs.indeed.com/api/conversion-tracking-api/conversion-tracking-api)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Employer Registration API

Part of the Candidate Sync APIs, this API allows ATS partners to register employers for Candidate Sync integration.

- **Human URL:** [https://docs.indeed.com/candidate-sync-apis/employer-registration-api/](https://docs.indeed.com/candidate-sync-apis/employer-registration-api/)
- **Base URL:** `https://apis.indeed.com/graphql`

#### Tags

- ATS
- Candidate Sync
- Employers
- Registration

#### Properties

- [Documentation](https://docs.indeed.com/candidate-sync-apis/employer-registration-api/)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Indeed Retrieve Candidates API

Part of the Candidate Sync APIs, this API enables ATS partners to get candidate and application information from Indeed on behalf of employers.

- **Human URL:** [https://docs.indeed.com/candidate-sync-apis/retrieve-candidates-api/](https://docs.indeed.com/candidate-sync-apis/retrieve-candidates-api/)
- **Base URL:** `https://apis.indeed.com/graphql`

#### Tags

- Applications
- ATS
- Candidate Sync
- Candidates

#### Properties

- [Documentation](https://docs.indeed.com/candidate-sync-apis/retrieve-candidates-api/)
- [JSON Schema](json-schema/indeed-candidate-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](json-ld/indeed-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [Postman Collection](collections/indeed-employer-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/indeed-employer-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Developer Portal](https://opensource.indeedeng.io/)
- [Terms of Service](https://www.indeed.com/legal/terms-of-service)
- [Privacy Policy](https://www.indeed.com/legal/privacy)
- [Status Page](https://status.indeed.com)
- [Support](https://support.indeed.com/hc/en-us)
- [Blog](https://engineering.indeedblog.com/)
- [GitHub Organization](https://github.com/indeedeng)
- [X (Twitter)](https://twitter.com/indeedeng)
- [Rate Limits](https://opensource.indeedeng.io/api-documentation/docs/rate-limits)
- [Authentication](https://docs.indeed.com/authorization/)
- [Getting Started](https://docs.indeed.com/getstarted/)
- [Release Notes](https://docs.indeed.com/release-notes/)
- [Sandbox](https://docs.indeed.com/getstarted/simulated-graphql-environment)
- [LinkedIn](https://www.linkedin.com/company/indeed-com/)
- [Features](undefined)
- [Use Cases](undefined)
- [Integrations](undefined)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
