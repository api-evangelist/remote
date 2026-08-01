---
name: Subscribe to Remote webhooks
description: Register, test, and verify webhook callbacks for employment events via the Remote API.
api: openapi/remote-openapi-original.json
operations: [post_v1_webhook-callbacks, get_v1_companies_company_id_webhook-callbacks, post_v1_sandbox_webhook-callbacks_trigger, delete_v1_webhook-callbacks_id]
---

# Subscribe to Remote webhooks

Get notified of employment lifecycle events instead of polling. Webhooks are
company-scoped, so use a company-scoped OAuth 2.0 Bearer token with
`webhook:read` / `webhook:write` scopes.

## Steps

1. **Register a callback.** Call `post_v1_webhook-callbacks` with
   `subscribed_events` (e.g. `employment.onboarding.completed`,
   `payslip.released`) and a unique `url`. The response returns a `signing_key`
   — store it securely; it is used to verify inbound requests.
2. **List callbacks.** Call `get_v1_companies_company_id_webhook-callbacks` to
   confirm what is registered for the company.
3. **Test in sandbox.** Call `post_v1_sandbox_webhook-callbacks_trigger` to fire
   a synthetic event and validate your endpoint end to end.
4. **Verify each delivery.** Recompute the HMAC signature from the `signing_key`
   and compare to the `X-Remote-Signature` header; use `X-Remote-Timestamp`
   (unix ms) to reject stale requests.
5. **Remove a callback.** Call `delete_v1_webhook-callbacks_id` when no longer
   needed (deleting and re-registering rotates the signing key).

## Conventions

- **Scopes:** `webhook:read`, `webhook:write`.
- **Signature headers:** `X-Remote-Signature`, `X-Remote-Timestamp`.
- **Catalog:** full event list in asyncapi/remote-webhooks.yml.
- **Rate limit:** 300 req/min per company.
