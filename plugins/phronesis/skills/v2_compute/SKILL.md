---
name: phronesis-compute-forecasting
description: Request, read, and verify compute and AI-infrastructure forecasts from the Phronesis platform — chip supply chains, datacenter build-out, training-compute demand, and inference-cost trajectories. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about compute capacity rather than a free-text guess.
---

# Phronesis — Compute & AI Infrastructure Forecasting (V2)

Phronesis V2 (Compute + AI Infrastructure) forecasts the build-out of the compute
substrate behind AI: chip supply, datacenter capacity, and the demand and cost
curves for training and inference. Use this skill when an agent must ground a
capacity, procurement, or capital-allocation decision in a calibrated forecast.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V2 — Compute + AI Infrastructure — Pythia ring `Pythia-Compute`
- **Note:** V2 dispatches through the energy/intersection cluster at intake (no
  dedicated Themis cluster — cross-vertical reuse). It is naturally adjacent to V1
  (Energy) and V9 (AI/AGI); consider the `cross_product` skill for those crossings.
- **Transports:** co-equal MCP and REST.

## Archetypes available

- `chip-supply-chain` — semiconductor supply, foundry capacity, lead times.
- `datacenter-buildout` — datacenter capacity additions by region / operator.
- `training-compute-demand` — demand for frontier-model training compute.
- `inference-cost-trajectory` — cost-per-token / cost-per-inference curves.

Confirm the archetype is live via `GET /v1/catalog` (the V2 entry's
`archetypes_available` list and `status`).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V2",
  "archetype": "datacenter-buildout",
  "subject": "compute",
  "question": "US datacenter capacity additions in GW for 2027",
  "horizon": "2027",
  "compute_tier": "deep"
}
```

`compute_tier` is abstract: `standard`, `deep`, `strategic`, or `strategic-sync`.

## Reading the Decision API output

The response is a contract-v1 envelope `{ "status": "ok", "data": {...},
"request_id": "..." }`. The `data` decision forecast contains:

- **Point forecast** — `p50` value with units.
- **Uncertainty band** — monotonic `p10 / p50 / p90`; present the band, not the point.
- **Assumptions** — enumerated, each with an id.
- **Sources** — the citation chain.
- **Sensitivity** — drivers, each referencing an assumption id.
- **Cost attestation** — attested per-call cost.
- **`audit_trail_id`** — for fetching the Trust Receipt.

## Verifying a forecast

1. `GET /v1/trust/receipt/{forecast_id}` — public signed Trust Receipt.
2. `GET /calibration/leaderboard` — check the V2 (Compute) accuracy row vs baselines.
3. `GET /v1/substrate/completeness` — V2 substrate-completeness attestation.

## Guidance for agents

- Compute forecasts are tightly coupled to energy; if your decision depends on power
  availability, request both V1 and V2 or use the `cross_product` skill.
- Pass the full uncertainty band downstream and cite the Trust Receipt id.
