---
name: Run an AI phishing simulation campaign
description: Authenticate, pick a simulation template, schedule an AI phishing campaign against one or more users, then read back status and results.
api: openapi/hacware-openapi.yml
operations: [PostAPIAuth, GetPhishSimulationsList, PostDirectPhish, PostPhishStatus, GetPhishResponsesReport]
---

# Run an AI phishing simulation campaign

Use the HacWare Security Awareness API to launch and measure an AI-generated phishing simulation.

## Prerequisites
- An `appid` and secret key (`sec`) issued to your HacWare partner/company account.
- Your tenant subdomain (the API host is `https://{domain}`, assigned at onboarding).

## Steps
1. **Authenticate** — `POST /api/v1/auth/` (`PostAPIAuth`) with `appid` and `sec`. This returns an access token and a refresh token. Send the access token on every subsequent call as `Authorization: Bearer <access_token>`. Tokens expire ~1 hour after issue; renew with the refresh token rather than re-exchanging the secret each time.
2. **List available simulations** — `GET /api/v1/phish/simulations-list/` (`GetPhishSimulationsList`) to choose a phishing template. Custom templates are available via the custom-simulation operations.
3. **Schedule the campaign** — `POST /api/v1/phish/directphish/` (`PostDirectPhish`) with a JSON body `{"data":{"content":[{"email":"user@company.com","phish_type":"Clickable Link","scheduled_date":"..."}]}}`. `phish_type` is one of `Clickable Link`, `Attachment`, `Direct Response`, or `SMS`; `scheduled_date` is optional.
4. **Check delivery status** — `GET /api/v1/phish/getperformers/` (`PostPhishStatus`) to confirm the campaign is processing/sent.
5. **Read results & metrics** — `GET /api/v1/phish/responses_report/` (`GetPhishResponsesReport`) for campaign responses and metrics (who clicked, failure counts, timing).

## Conventions & error handling
- Auth: bearer token; see `authentication/hacware-authentication.yml`.
- Errors return `{"message": "..."}` on 4xx (see `errors/hacware-problem-types.yml`); a 401 means the token is missing/expired — refresh and retry.
- Usage is metered by monthly API credits per plan (see `conventions/hacware-conventions.yml`).
- No idempotency-key is supported, so avoid blind retries of `PostDirectPhish` to prevent duplicate campaigns.
