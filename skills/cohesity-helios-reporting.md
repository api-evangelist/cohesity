---
name: Pull Cohesity Helios reports
description: Discover report types, preview a report, run it on a schedule and export the artifact from the Helios Reporting service.
api: openapi/cohesity-helios-reporting-openapi.yml
generated: '2026-09-05'
method: generated
source: openapi/cohesity-helios-reporting-openapi.yml
operations:
  - GetReports
  - GetReportById
  - GetReportType
  - GetReportPreview
  - GetComponents
  - GetComponentPreview
  - GetSchedules
  - CreateSchedule
  - UpdateSchedule
  - RunScheduleOnDemand
  - ExportReport
  - GetTasks
  - GetTaskById
  - GetArtifactForTask
---

# Pull Cohesity Helios reports

The Helios Reporting service is a separate API from the cluster surface, based at
`https://helios.cohesity.com/heliosreporting/api/v1`, authenticated with the same
`apiKey` header.

## Read path (start here)

1. `GetReports` — the catalog of available reports.
2. `GetReportById` / `GetReportType` — the definition and its parameters.
3. `GetReportPreview` — run it now and get rows back without creating anything.
   This is the operation to reach for when a user asks a one-off question; it
   creates no schedule and no artifact to clean up.
4. `GetComponents` / `GetComponentPreview` — individual charts/tables inside a
   report, when you want one number rather than the whole document.

## Scheduled / export path

1. `GetSchedules` before `CreateSchedule` — there is no idempotency key, so a
   retried create produces a duplicate recurring schedule that will keep emailing
   people.
2. `CreateSchedule`, then `UpdateSchedule` / `UpdateSchedulesState` to change or
   pause it.
3. `RunScheduleOnDemand` to fire an existing schedule once.
4. `ExportReport` produces a task; poll `GetTasks` / `GetTaskById`, then fetch
   the output with `GetArtifactForTask`.

## Notes

- Time windows are microseconds (`*Usecs`).
- Errors come back as `{errorCode, errorMessage}` on a `default` response; no
  status codes are enumerated.
- Pagination on the cluster surface uses an opaque `paginationCookie`; reporting
  operations use their own parameters — read the operation, do not assume.
