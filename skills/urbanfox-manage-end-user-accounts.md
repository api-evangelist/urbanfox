---
name: Manage end-user accounts
description: Inspect, create, block/unblock, link, and delete end-user accounts in
  UrbanFox.
api: openapi/urbanfox-customer-api-openapi.yml
operations: [getEndUserAccounts, getEndUserAccount, postEndUserAccount,
  putEndUserAccount, deleteEndUserAccount]
generated: '2026-07-21'
method: generated
---

# Manage end-user accounts

1. **List accounts** — `getEndUserAccounts` (`GET /v2/{tenant_slug}/enduseraccounts`),
   optional `is_blocked` filter. Scope: `read:enduseraccounts`.
2. **Inspect one** — `getEndUserAccount`
   (`GET /v2/{tenant_slug}/enduseraccounts/{website_account_id}`). Scope:
   `read:enduseraccount`. Key flags: `is_blocked`, `is_quarantined`,
   `is_fraudulent`; `crm_account_id` links the account to your CRM.
3. **Create** — `postEndUserAccount` (`POST /v2/{tenant_slug}/enduseraccounts`).
   Scope: `create:enduseraccount`. `409` type `item-already-exists` means the
   `website_account_id` is taken — fetch the existing account instead.
4. **Block / unblock / link** — `putEndUserAccount`
   (`PUT /v2/{tenant_slug}/enduseraccounts/{website_account_id}`) updating
   `is_blocked` / `is_quarantined` / `is_fraudulent` / `crm_account_id`. Scope:
   `update:enduseraccount`.
5. **Delete** — `deleteEndUserAccount`
   (`DELETE /v2/{tenant_slug}/enduseraccounts/{website_account_id}`). Scope:
   `delete:enduseraccount`. Returns `204`; deletion is irreversible — prefer
   blocking unless removal is required.

## Rules

- Write operations are NOT idempotent-keyed; check current state with
  `getEndUserAccount` before mutating, and treat `409` as "already exists", not a
  retryable failure.
- A `403` type `access-denied` usually means the token is missing the specific
  scope for that operation — scopes follow `action:resource` and are additive.
