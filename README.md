# Cohesity (cohesity)

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

Cohesity is a data security and management company providing backup, disaster recovery, archive, and cyber resilience capabilities across on-premises, cloud, and SaaS workloads. Following the merger with Veritas, the combined company protects enterprise data while powering automation, orchestration, and AI-driven recovery. The Cohesity developer surface centers on the Cohesity REST API, exposed both per-cluster (DataProtect/cluster API) and via the Helios global control plane, with versioned v1 and v2 endpoints, API-key authentication, and SDKs for Python, PowerShell, Ansible, Terraform, and ServiceNow.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/cohesity/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consuming
- **Access:** 3rd-Party

## Tags

- Automation
- Backup
- Cyber Resilience
- Data Management
- Data Protection
- Data Security
- DataProtect
- Disaster Recovery
- Helios
- Orchestration

## Timestamps

- **Created:** 2025-03-01
- **Modified:** 2026-04-28

## APIs

### Cohesity Helios REST API
Helios is Cohesity's SaaS-based control plane that manages a fleet of Cohesity clusters from a single global pane of glass. The Helios REST API authenticates via an apiKey generated from the Helios UI and passed in the request header, supports both v1 and v2 endpoint families, and lets customers list and operate registered clusters, run protection jobs, manage policies, and access reporting and analytics globally.

**Human URL:** [https://developer.cohesity.com/helios-api.html](https://developer.cohesity.com/helios-api.html)

**Base URL:** `https://helios.cohesity.com/irisservices/api/v1/public/`

#### Tags

- Automation, Cluster Management, DataProtect, Helios, Orchestration

#### Properties

- [Documentation](https://developer.cohesity.com/helios-api.html)
- [Getting Started](https://developer.cohesity.com/docs/helios-getting-started)
- [API Reference](https://developer.cohesity.com/apidocs/versions/)

### Cohesity DataProtect REST API
The DataProtect REST API is the per-cluster RESTful interface exposed by every Cohesity cluster, providing programmatic control over data management operations including backups, restores, replication, archival, cloud tiering, and storage domain management. Authentication is performed via API keys for automation users or username/password for interactive sessions.

**Human URL:** [https://developer.cohesity.com/apidocs/versions/](https://developer.cohesity.com/apidocs/versions/)

#### Tags

- Backup, DataProtect, Disaster Recovery, Per-Cluster

#### Properties

- [Documentation](https://developer.cohesity.com/apidocs/versions/)
- [API Reference](https://developers.cohesity.com/page/api-reference)

## Common Properties

- [Website](https://www.cohesity.com/)
- [Developer Portal](https://developer.cohesity.com/)
- [Developers Site](https://developers.cohesity.com/)
- [API Reference](https://developer.cohesity.com/apidocs/versions/)
- [GitHub](https://github.com/cohesity)
- [Documentation](https://docs.cohesity.com/)
- [Support](https://www.cohesity.com/support/)
- [Status](https://status.cohesity.com/)
- [Privacy Policy](https://www.cohesity.com/legal/privacy-policy/)
- [Terms of Use](https://www.cohesity.com/agreements/website-terms-of-use/)

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
