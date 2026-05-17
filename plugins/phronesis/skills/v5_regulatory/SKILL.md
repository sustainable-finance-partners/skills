---
name: phronesis-regulatory-forecasting
description: Request, read, and verify regulatory-compliance forecasts from the Phronesis platform — regulatory-shift forecasting, state RPS implementation, and federal IRA tax-credit utilization. A cross-cutting vertical that pairs with every other domain. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about regulatory change.
---

# Phronesis — Regulatory Forecasting (V5)

Phronesis V5 (Regulatory) forecasts regulatory and policy change: the likelihood and
timing of regulatory shifts, the pace of state-level Renewable Portfolio Standard
(RPS) implementation, and federal Inflation Reduction Act (IRA) tax-credit
utilization. V5 is **cross-cutting** — regulatory exposure conditions decisions in
energy, climate, healthcare, AI/AGI, and beyond.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V5 — Regulatory — Pythia ring `Pythia-Regulatory`, Themis cluster `themis-V5-regulatory`
- **Transports:** co-equal MCP and REST.

## Archetypes available

- `regulatory-shift-forecasting` — likelihood / timing of a regulatory change.
- `state-rps-implementation` — pace of state Renewable Portfolio Standard rollout.
- `federal-ira-tax-credit-utilization` — projected IRA tax-credit uptake.

Confirm via `GET /v1/catalog` (V5 entry).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V5",
  "archetype": "regulatory-shift-forecasting",
  "subject": "regulatory",
  "question": "Probability the EPA finalizes a stricter power-plant emissions rule before end of 2027",
  "horizon": "2027",
  "jurisdiction": "US-Federal",
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
2. `GET /calibration/leaderboard` — check the V5 (Regulatory) accuracy row.
3. `GET /v1/substrate/completeness` — V5 substrate-completeness attestation.

## Guidance for agents

- Because V5 is cross-cutting, regulatory risk is often best requested as an
  intersection — see the `cross_product` skill for crossings like AI/AGI x Regulatory
  or Energy x Regulatory.
- Always name the jurisdiction; regulatory forecasts are jurisdiction-specific.
- Pass the full band and assumptions downstream; regulatory timing is high-variance.
