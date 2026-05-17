---
name: phronesis-robotics-forecasting
description: Request, read, and verify robotics forecasts from the Phronesis platform — manipulation-capability progression, labor-displacement, and physical-deployment trajectories. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about robotics safety and deployment rather than a free-text guess.
---

# Phronesis — Robotics Forecasting (V8)

Phronesis V8 (Robotics) forecasts robotics safety and deployment: the progression of
manipulation capability, the pace and pattern of labor displacement, and real-world
physical deployment trajectories. Use this skill when an agent must ground a
robotics capability, workforce, or deployment decision in a calibrated forecast.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V8 — Robotics — Pythia ring `Pythia-Robotics`, Themis cluster `themis-V8-robotics`
- **Transports:** co-equal MCP and REST.

## Archetypes available

- `manipulation-capability` — progression of robotic manipulation / dexterity.
- `labor-displacement` — pace and sectoral pattern of automation-driven displacement.
- `physical-deployment` — real-world robot deployment counts / trajectories.

Confirm via `GET /v1/catalog` (V8 entry's `archetypes_available` + `status`).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V8",
  "archetype": "physical-deployment",
  "subject": "robotics",
  "question": "Number of humanoid robots deployed in US warehouse operations by end of 2028",
  "horizon": "2028",
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
2. `GET /calibration/leaderboard` — check the V8 (Robotics) accuracy row.
3. `GET /v1/substrate/completeness` — V8 substrate-completeness attestation.

## Guidance for agents

- Labor-displacement forecasts pair with V5 (Regulatory) and V9 (AI/AGI); see the
  `cross_product` skill for those crossings.
- Pass the full uncertainty band downstream and cite the Trust Receipt id.
