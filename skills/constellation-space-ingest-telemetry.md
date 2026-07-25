---
name: Ingest ground telemetry
description: Push ground telemetry metrics into ConstellationOS, aligned to the fleet topology graph.
api: openapi/constellation-space-openapi.yml
operations:
- ingestTelemetry
---

# Ingest ground telemetry

Stream ground telemetry into ConstellationOS so predictions run on live fleet context.

## Auth
- Send `Authorization: Bearer <token>` (env var `CONSTELLATION_API_TOKEN`).
- This flow needs the `telemetry:write` scope.
- Base URL: `https://api.constellation.space` (non-prod: `https://api-dev.constellation.space`).

## Steps
1. **ingestTelemetry** — `POST /api/v1/telemetry/ingest` with metrics in line
   protocol. The first-party fleet agent (`fleet-agent.mjs telemetry post`) is the
   reference producer; it posts metrics via stdin or `--file`.

## Rules
- Metrics are aligned to the fleet graph — read topology first (`getTopology`) if you
  need node/link identifiers.
- `401` = missing/invalid/revoked token; `403` = token missing `telemetry:write`.
- Use a `cos_dev_` token against the dev host to validate before writing to production.
- See `cli/constellation-space-cli.yml` and `sandbox/constellation-space-sandbox.yml`.
