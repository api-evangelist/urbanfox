---
name: Investigate and close a fraud case
description: Find open fraud cases, review risk details and model explanations,
  and close resolved cases in UrbanFox.
api: openapi/urbanfox-customer-api-openapi.yml
operations: [getAllCases, getCase, putCase]
generated: '2026-07-21'
method: generated
---

# Investigate and close a fraud case

1. **Find open cases** — `getAllCases` (`GET /v2/{tenant_slug}/cases?status=OPEN`).
   Scope: `read:cases`. Each item carries `case_id`, `website_account_id`,
   `created_at`, `status`, `overall_risk_score`.
2. **Open one case** — `getCase` (`GET /v2/{tenant_slug}/cases/{case_id}`).
   Scope: `read:case`. Review `overall_risk_score`, `primary_fraud_label`,
   `recommended_action`, and the per-occurrence `explanation`
   (`fraud_reason`, `fraud_types`, `model_behaviour`).
3. **Close it when resolved** — `putCase` (`PUT /v2/{tenant_slug}/cases/{case_id}`)
   with body `{"status": "CLOSED"}`. Scope: `update:case`.

## Rules

- `404` with type `item-not-found` means the `case_id` does not exist for this
  tenant — verify the ID before retrying.
- `422` validation errors return `errors[]` of `{field, message}` — surface them
  rather than blind-retrying.
- Cases reference end-user accounts via `website_account_id`; use the
  manage-end-user-accounts skill to block the account behind a confirmed-fraud
  case.
