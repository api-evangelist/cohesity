---
name: Recover data with Cohesity and reverse it if wrong
description: Find a snapshot, start a recovery, watch it, and cancel or tear it down when it was the wrong one — the reversal-aware path through the Cohesity v2 recovery API.
api: openapi/cohesity-cluster-v2-openapi.yml
generated: '2026-09-05'
method: generated
source: openapi/cohesity-cluster-v2-openapi.yml + conventions/cohesity-conventions.yml
operations:
  - GetProtectedObjectsOfAnyType
  - GetObjectSnapshots
  - GetPITRangesForProtectedObject
  - CreateRecovery
  - GetRecoveries
  - GetRecoveryById
  - CancelRecoveryById
  - TearDownRecoveryById
  - CreateDownloadFilesAndFoldersRecovery
---

# Recover data with Cohesity — and reverse it if wrong

Recovery is the highest-consequence thing this API does. Read the reversal
section **before** calling `CreateRecovery`, not after.

## Reversal contract (read first)

| Forward action | Reversal | Window |
|---|---|---|
| `CreateRecovery` (in flight) | `CancelRecoveryById` | while running; no window published |
| `CreateRecovery` (completed) | `TearDownRecoveryById` | no window published |

Cohesity publishes **no time window** for either reversal. Treat "in progress" as
the only reliable cancellation window and confirm state with `GetRecoveryById`
rather than assuming. There is no dry-run mode on any recovery operation.

## Steps

1. **Identify the object.** `GetProtectedObjectsOfAnyType`, filtered by `ids` or
   `names`. Note the object id — on Helios these are frequently composite
   (`clusterId:clusterIncarnationId:objectId`).
2. **Choose a point in time.** `GetObjectSnapshots` lists snapshots;
   `GetPITRangesForProtectedObject` gives the continuous ranges where
   point-in-time recovery is possible. Time filters are **microseconds** (`*Usecs`).
3. **Pick the smallest recovery that solves the problem.** If the user needs
   files rather than a whole VM or database, use
   `CreateDownloadFilesAndFoldersRecovery` instead of a full `CreateRecovery` —
   it is materially less disruptive.
4. **Start the recovery.** `CreateRecovery`, with the environment-specific
   recover parameter block and a recover target. Capture the returned recovery id
   immediately — without it you cannot reverse.
5. **Watch it.** Poll `GetRecoveryById`. Do not re-issue `CreateRecovery` on a
   timeout: there is no idempotency key, and a retry starts a **second**
   recovery.
6. **Reverse if wrong.** `CancelRecoveryById` while in flight;
   `TearDownRecoveryById` to remove the resources a completed recovery created.

## Guardrails for an agent

- Never call `CreateRecovery` against a production target without an explicit
  human confirmation of object, snapshot time and recover target.
- A failed poll is not a failed recovery. Re-read; do not re-write.
- `GetRecoveries` is the audit surface — list before creating so you never start
  a recovery that is already running.
