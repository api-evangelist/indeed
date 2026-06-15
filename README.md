# Indeed (indeed)

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
