---
name: Manage time off with Remote
description: Create, approve, and check balances for employee time-off requests via the Remote API.
api: openapi/remote-openapi-original.json
operations: [get_v1_timeoff_types, get_v1_timeoff-balances_employment_id, post_v1_timeoff, post_v1_timeoff_timeoff_id_approve]
---

# Manage time off with Remote

Handle the time-off lifecycle for an employment. Requires a company-scoped
OAuth 2.0 Bearer token with `timeoff:read` / `timeoff:write` scopes.

## Steps

1. **List leave types.** Call `get_v1_timeoff_types` to get the valid time-off
   types for the employment's country/policy.
2. **Check the balance.** Call `get_v1_timeoff-balances_employment_id` with the
   `employment_id` to confirm the employee has enough balance.
3. **Create the request.** Call `post_v1_timeoff` with the employment, type, and
   dates. This returns a `timeoff_id`.
4. **Approve it.** Call `post_v1_timeoff_timeoff_id_approve` with the
   `timeoff_id` to approve the request.

## Conventions

- **Scopes:** `timeoff:read`, `timeoff:write` (see scopes/remote-scopes.yml).
- **Events:** subscribe to `timeoff.requested` / `timeoff.approved` webhooks to
  react to lifecycle changes (asyncapi/remote-webhooks.yml).
- **Rate limit:** 300 req/min per company; respect `x-ratelimit-*` headers.
- **Errors:** `422` on validation failure; `403` if the token lacks the scope.
