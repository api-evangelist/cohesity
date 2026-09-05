---
name: Subscribe to Cohesity alerts via webhook
description: Turn Cohesity's alert engine into an event feed by creating a notification rule with a webhook delivery target, scoped by category and severity.
api: openapi/cohesity-cluster-v1-openapi.yml
generated: '2026-09-05'
method: generated
source: openapi/cohesity-cluster-v1-openapi.yml + asyncapi/cohesity-webhooks.yml
operations:
  - GetAlertCategories
  - GetAlertTypes
  - GetAlerts
  - GetAlertById
  - GetNotificationRules
  - CreateNotificationRule
  - UpdateNotificationRule
  - DeleteNotificationRule
  - GetActiveAlertsStats
---

# Subscribe to Cohesity alerts via webhook

Cohesity has no event bus and no AsyncAPI. It has an **alert engine plus a rule
engine**: you subscribe by creating a `NotificationRule` that matches alerts and
attaches delivery targets, one of which is an outbound HTTP callback. The
subscription is a REST write on the v1 surface.

## Steps

1. **Learn the vocabulary.** `GetAlertCategories` and `GetAlertTypes`. Categories
   are a fixed enum (`kSecurity`, `kBackupRestore`, `kClusterHealth`, `kQuota`,
   `kEncryption`, `kAntivirus`, `kRemoteReplication`, …); alert types are integers
   resolved at runtime, so never hard-code one.
2. **Check what already exists.** `GetNotificationRules`. Duplicate rules mean
   duplicate deliveries — there is no idempotency key on
   `CreateNotificationRule`.
3. **Create the rule.** `CreateNotificationRule` with:
   - `categories` — the enum values you actually want. Do **not** subscribe to
     everything; `kInfo` across all categories is a firehose.
   - `severities` — `kCritical`, `kWarning`, `kInfo`.
   - `alertTypeList` — optional narrowing by type id.
   - `webHookDeliveryTargets` — `externalApiUrl` (your endpoint) plus optional
     `curlOptions`.
4. **Verify** with `GetNotificationRules` and confirm exactly one rule matches
   your intent.
5. **Adjust or remove** with `UpdateNotificationRule` / `DeleteNotificationRule`.

## What you must build yourself

Cohesity's webhook delivery is a server-side curl invocation. It publishes:

- **no signing secret and no signature header** — you cannot cryptographically
  verify a callback came from Cohesity. Authenticate at your endpoint some other
  way (mTLS, a secret path segment, an allowlisted source).
- **no delivery id and no retry policy** — assume at-most-once and reconcile.
- **no payload schema** — the reliable pattern is to treat the callback as a
  *doorbell* and immediately re-read state with `GetAlerts` / `GetAlertById`.

`GetActiveAlertsStats` is a cheap poll if you would rather not accept inbound
traffic at all.
