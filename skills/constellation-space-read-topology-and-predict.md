---
name: Read fleet topology and run a link forecast
description: Pull the current ConstellationOS fleet topology, then run an ML link forecast over that live fleet context.
api: openapi/constellation-space-openapi.yml
operations:
- getTopology
- runPredictions
---

# Read fleet topology and run a link forecast

Use the ConstellationOS API to read the live fleet graph and forecast link health.

## Auth
- Send `Authorization: Bearer <token>` on every request. Tokens are minted in the
  console (Settings -> API) and are scoped and shown once. Store as
  `CONSTELLATION_API_TOKEN`. Production tokens are prefixed `cos_live_`,
  non-production `cos_dev_`.
- This flow needs the `topology:read` and `predictions:run` scopes.
- Base URL: `https://api.constellation.space` (non-prod: `https://api-dev.constellation.space`).

## Steps
1. **getTopology** — `GET /api/v1/topology/` to retrieve the fleet graph snapshot
   (nodes, links, health scores, routing state).
2. **runPredictions** — `POST /api/v1/predictions/` to forecast link conditions
   (SNR/link fade, traffic, weather, jamming, conjunction) for the node/link/time
   context of interest.

## Rules
- A `401` means the token is missing, invalid, or revoked (tokens are validated on
  every request). A `403` means the token lacks the required scope — mint a token
  with `topology:read` + `predictions:run`.
- No idempotency key is documented; treat `runPredictions` as a plain POST.
- See `conventions/constellation-space-conventions.yml` and
  `errors/constellation-space-problem-types.yml`.
