---
name: Query tenant activity metrics
description: Retrieve aggregated session and event metrics for a date range from
  UrbanFox.
api: openapi/urbanfox-customer-api-openapi.yml
operations: [getMetricsAggregate]
generated: '2026-07-21'
method: generated
---

# Query tenant activity metrics

1. **Fetch metrics** — `getMetricsAggregate` (`GET /v2/{tenant_slug}/metrics`).
   Scope: `read:metrics`. Required: `start_date`, `end_date` (ISO 8601 with
   timezone, e.g. `2024-01-01T00:00:00Z`). Optional: `aggregation`
   (`minute` max 7-day range, `hour` max 90-day range, `day`/`week`/`month`/
   `quarter`/`year` unlimited; omit for non-aggregated points) and `metric` to
   return a single field (e.g. `total_events`, `unique_sessions`).
2. **Compare periods** — issue one request per window with the same `metric` and
   `aggregation`, then diff the returned `metrics` arrays.

## Rules

- `400` "end_date must be after start_date" -> swap the values; `400`
  validation-error -> check ISO 8601 formatting.
- An empty `metrics` array is valid (no data in the window) — widen the range
  before assuming an error.
- Fields per data point: `total_events`, `unique_sessions`, `guest_sessions`,
  `non_guest_sessions`, `session_starts`, `unique_ids`.
