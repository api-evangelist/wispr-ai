---
name: Check Wispr Flow backend health and user status
description: Probe the Wispr Flow backend service health, deployed version, and the
  signed-in user's account status before running any dictionary or history flows.
api: openapi/wispr-ai-backend-openapi-original.json
operations:
- health
- version
- user_status
generated: '2026-07-21'
method: generated
---

# Check Wispr Flow backend health and user status

Use these unauthenticated service endpoints on `https://api.wisprflow.ai` before
driving any authenticated flow, and the user-status check right after auth.

## Steps

1. **Service liveness** — `health` (`GET /health`). The root (`GET /`) also returns
   `{"status": "ready"}` when the service is up.
2. **Deployed version** — `version` (`GET /version`) to log the backend version
   you are talking to (spec `info.version` was `0.5.2` when harvested).
3. **Account state** — `user_status` (`GET /user_status`) to confirm the user's
   sign-in and entitlement state before dictionary/history/notes calls.

## Notes

- `health`, `version`, and `user_status` declare no security requirement in the
  spec; everything under `/api/v1/` requires the `Authorization` API-key header
  (see `authentication/wispr-ai-authentication.yml`).
- Status page for incidents: https://statuspage.incident.io/wispr-flow
