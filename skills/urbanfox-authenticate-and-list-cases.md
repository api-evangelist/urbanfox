---
name: Authenticate and list fraud cases
description: Obtain a machine-to-machine token and make a first authenticated call
  to list fraud cases for a UrbanFox tenant.
api: openapi/urbanfox-customer-api-openapi.yml
operations: [postOAuthToken, getAllCases]
generated: '2026-07-21'
method: generated
---

# Authenticate and list fraud cases

1. **Get a token** — `postOAuthToken` (`POST https://api.{tenant_slug}.urbanfox.io/v2/oauth/token`).
   Send `application/x-www-form-urlencoded` (NOT JSON): `grant_type=client_credentials`,
   `client_id`, `client_secret`. The response returns `access_token` (Bearer JWT),
   `token_type`, and `expires_in` (86400 s). Renew before expiry; treat a `401` on
   any call as a renewal signal.
2. **List cases** — `getAllCases` (`GET /v2/{tenant_slug}/cases`) with
   `Authorization: Bearer <access_token>`. Requires scope `read:cases`. Optional
   filters: `status` (`OPEN`|`CLOSED`), `website_account_id`, `user_is_blocked`.
   There is no pagination — lists return filtered results only.

## Rules

- The same `tenant_slug` appears in the host (`api.{tenant_slug}.urbanfox.io`) and
  the path (`/v2/{tenant_slug}/...`). Use the identical value in both.
- Errors are RFC 7807 problem details served as `application/json`; route on the
  `type` slug (`unauthorized` -> re-authenticate, `access-denied` -> missing scope
  or tenant mismatch). See `errors/urbanfox-problem-types.yml`.
- No idempotency keys and no documented rate-limit headers; retry `500` with
  exponential backoff, `502` after a short delay.
- Never expose `client_secret` in client-side code, logs, or third-party tools.
