---
name: Onboard a partner and create a customer (multi-tenant)
description: Use the tiered multi-tenant API to authenticate, invite a partner, create a customer under that partner, and list customers.
api: openapi/hacware-openapi.yml
operations: [PostTieredAPIAuth, PostInvitePartner, AddCustomer, CustomerList]
---

# Onboard a partner and create a customer (multi-tenant)

HacWare's tiered API (`/api/dev/tier/`) models an MSP/reseller hierarchy: partners own customers.

## Steps
1. **Authenticate to the tiered API** — `POST /api/dev/tier/auth/` (`PostTieredAPIAuth`) to obtain a bearer token scoped to the tiered/partner surface.
2. **Create & invite a partner** — `POST /api/dev/tier/partners` (`PostInvitePartner`) to create a partner and invite its admin. Capture the returned `partner_id`.
3. **Create a customer under the partner** — `POST /api/dev/tier/partners/{partner_id}/customers/` (`AddCustomer`) to provision a customer organization and its subscription.
4. **List customers** — `GET /api/dev/tier/partners/{partner_id}/customers/` (`CustomerList`) to confirm the customer exists and read its subscription state.

## Conventions & error handling
- Bearer auth (`authentication/hacware-authentication.yml`); tokens expire ~1 hour.
- 4xx errors carry `{"message": "..."}` (`errors/hacware-problem-types.yml`).
- Subscriptions are updated/cancelled via `UpdateCustomerSubscription` (PATCH) and `CancelCustomerSubscription` (DELETE) on the same customer path.
- The partner/customer graph is described in `data-model/hacware-data-model.yml`.
