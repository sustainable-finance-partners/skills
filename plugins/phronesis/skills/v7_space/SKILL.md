---
name: phronesis-space-forecasting
description: Request, read, and verify space-economy forecasts from the Phronesis platform — launch cadence, satellite-constellation build-out, and disaster monitoring. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about the space economy rather than a free-text guess.
---

# Phronesis — Space Forecasting (V7)

Phronesis V7 (Space) forecasts the space economy: orbital launch cadence, satellite
constellation build-out, and the use of space assets for disaster monitoring. Use
this skill when an agent must ground a space-sector capacity, deployment, or
capability decision in a calibrated forecast.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V7 — Space — Pythia ring `Pythia-Space`, Themis cluster `themis-V7-space`
- **Transports:** co-equal MCP and REST.

## Archetypes available

- `launch-cadence` — orbital launch rate by provider / region over a horizon.
- `satellite-constellation-buildout` — constellation deployment trajectories.
- `disaster-monitoring` — space-based disaster-monitoring capability / coverage.

Confirm via `GET /v1/catalog` (V7 entry's `archetypes_available` + `status`).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V7",
  "archetype": "launch-cadence",
  "subject": "space",
  "question": "Global orbital launch count for 2027",
  "horizon": "2027",
  "compute_tier": "deep"
}
```

`compute_tier`: `standard`, `deep`, `strategic`, or `strategic-sync`.

## Reading the Decision API output

Contract-v1 envelope `{ "status": "ok", "data": {...}, "request_id": "..." }`. The
`data` decision forecast contains a **point forecast** (`p50`), a monotonic
**uncertainty band** (`p10 / p50 / p90`), enumerated **assumptions**, the **sources**
citation chain, **sensitivity** drivers referencing assumption ids, the **cost
attestation**, and an **`audit_trail_id`**.

## Verifying a forecast

1. `GET /v1/trust/receipt/{forecast_id}` — public signed Trust Receipt.
2. `GET /calibration/leaderboard` — check the V7 (Space) accuracy row vs baselines.
3. `GET /v1/substrate/completeness` — V7 substrate-completeness attestation.

## Guidance for agents

- Pass the full uncertainty band downstream; never the point forecast alone.
- Cite the Trust Receipt `audit_trail_id` so the decision chain stays verifiable.
