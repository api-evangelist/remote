---
name: Onboard an employee with Remote
description: Estimate employment cost for a country and create an employment (EOR onboarding) via the Remote API.
api: openapi/remote-openapi-original.json
operations: [get_v1_countries, post_v1_cost-calculator_estimation, post_v1_employments]
---

# Onboard an employee with Remote

Use the Remote API to price and create a new global employment. All requests go
to `https://gateway.remote.com/v1/` (or `https://gateway.remote-sandbox.com/v1/`
in sandbox) and require an OAuth 2.0 Bearer access token (tokens live 3600s).

## Steps

1. **Pick the country.** Call `get_v1_countries` to list supported countries and
   get the `country_code` you need. Country data drives the dynamic, country-
   specific onboarding form fields (JSON Schema).
2. **Estimate the cost.** Call `post_v1_cost-calculator_estimation` with the
   country, salary, and contract details to get the full employment cost
   breakdown before committing.
3. **Create the employment.** Call `post_v1_employments` with the employee's
   basic information and contract details. This starts the EOR onboarding and
   returns an `employment_id` you use for every downstream call.

## Conventions

- **Auth:** OAuth 2.0 Bearer; company-scoped token for company operations.
- **Pagination:** `page` / `page_size` query params on list endpoints.
- **Rate limit:** 300 requests/minute per company; honor `x-ratelimit-*` headers
  and back off on `429`.
- **Errors:** validation failures return `422` (UnprocessableEntityResponse);
  see errors/remote-problem-types.yml.
- **Test first:** use `ra_test_` tokens against the sandbox host before going live.
