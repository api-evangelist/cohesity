---
name: Protect a workload with Cohesity
description: Register a protection source, find its objects, attach them to a protection group backed by a policy, and confirm the first run — using the Cohesity v2 REST API.
api: openapi/cohesity-cluster-v2-openapi.yml
generated: '2026-09-05'
method: generated
source: openapi/cohesity-cluster-v2-openapi.yml + conventions/cohesity-conventions.yml
operations:
  - GetProtectionSources
  - GetSourceRegistrations
  - GetProtectedObjectsOfAnyType
  - GetProtectionPolicies
  - CreateProtectionPolicy
  - CreateProtectionGroup
  - GetProtectionGroups
  - CreateProtectionGroupRun
  - GetProtectionGroupRuns
  - CancelProtectionGroupRun
---

# Protect a workload with Cohesity

Use this when the goal is "start backing up X on a schedule". Every operationId
below exists verbatim in `openapi/cohesity-cluster-v2-openapi.yml`.

## Before you start

- **Auth**: send a Helios or cluster API key in the `apiKey` header on every
  request. See `authentication/cohesity-authentication.yml`. If the account has
  MFA enabled, Cohesity states MFA does not support automation — use an API key
  created from a non-MFA automation identity.
- **Base URL**: `/v2` on the cluster, or `https://helios.cohesity.com/v2` through
  the Helios control plane.
- **Environment is a discriminator, not a label.** Almost every request body here
  is a union keyed on `environment` (`kVMware`, `kAWS`, `kO365`, `kSQL`,
  `kOracle`, `kPhysical`, …). Select the matching parameter block; there is no
  flat generic body.
- **There is no idempotency key.** A retried `CreateProtectionGroup` or
  `CreateProtectionGroupRun` creates or starts a second one. Never blind-retry a
  write — re-read with the corresponding `Get*` and reconcile.

## Steps

1. **Confirm the source is registered.** Call `GetSourceRegistrations`. If the
   target is not there, register it first (see the registration operations on
   `/data-protect/sources/registrations`) and wait for discovery to finish.
2. **List what is protectable.** Call `GetProtectionSources` for the hierarchy,
   then `GetProtectedObjectsOfAnyType` to see what is already covered. Set
   `tenantIds` / `includeTenants` deliberately if you operate in a
   service-provider tenancy — the defaults scope to the calling tenant only.
3. **Pick or create a policy.** `GetProtectionPolicies` first; reuse an existing
   policy whenever one matches the required retention and frequency. Only call
   `CreateProtectionPolicy` when nothing fits — policies are shared objects and
   proliferating them is a real operational cost.
4. **Create the protection group.** `CreateProtectionGroup` with the policy id,
   the object ids, and the environment-specific parameter block. Capture the
   returned group id.
5. **Verify, do not assume.** Call `GetProtectionGroups` (or
   `GetProtectionGroupById`) and confirm exactly one group exists for this
   intent. This is the step that substitutes for idempotency.
6. **Optionally kick a first run.** `CreateProtectionGroupRun`, then poll
   `GetProtectionGroupRuns` until the run leaves the in-progress state.
7. **If you need to stop it**, `CancelProtectionGroupRun` cancels an in-flight
   run. Cohesity publishes no time window for this — it works while the run is
   still running and not after. See `conventions/cohesity-conventions.yml`.

## Reading errors

There is no RFC 9457 problem document. Every operation returns a single `default`
error carrying `{errorCode, message}` in v2 (`{errorCode, errorMessage}` in v1 —
the two are not interchangeable). Statuses are not enumerated per operation, so
branch on the envelope, not on a documented status list. A `401` returns
`{"errorCode":"KStatusUnauthorized"}`.

## Timestamps

All time fields are **microseconds** since epoch and end in `Usecs`. Passing
milliseconds returns an empty result rather than an error.
