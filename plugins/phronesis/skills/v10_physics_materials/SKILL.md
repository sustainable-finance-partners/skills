---
name: phronesis-physics-materials-forecasting
description: Request, read, and verify physics and materials-discovery forecasts from the Phronesis platform — AI-driven materials discovery, physics-experimentation cadence, battery-chemistry breakthroughs, superconductor discovery, and advanced-manufacturing substrate. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about scientific discovery rather than a free-text guess.
---

# Phronesis — Physics & Materials Forecasting (V10)

Phronesis V10 (Physics & Materials Discovery) forecasts the pace of scientific
discovery: AI-driven materials discovery, physics-experimentation cadence,
battery-chemistry breakthroughs, superconductor discovery, and the maturation of
advanced-manufacturing substrate. Use this skill when an agent must ground a
research, R&D-investment, or technology-bet decision in a calibrated forecast.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V10 — Physics & Materials Discovery — Pythia ring `Pythia-Physics-Materials`, Themis cluster `themis-V10-physics-materials-#97`
- **Transports:** co-equal MCP and REST.

## Archetypes available

- `ai-materials-discovery` — pace of AI-accelerated materials discovery.
- `physics-experimentation-cadence` — throughput of physics experimentation.
- `battery-chemistry-breakthrough` — likelihood / timing of battery-chemistry advances.
- `superconductor-discovery` — probability / timing of superconductor milestones.
- `advanced-manufacturing-substrate` — maturation of advanced-manufacturing capability.

Confirm via `GET /v1/catalog` (V10 entry's `archetypes_available` + `status`).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V10",
  "archetype": "battery-chemistry-breakthrough",
  "subject": "physics_materials",
  "question": "Probability a solid-state battery chemistry reaches commercial-scale production by 2029",
  "horizon": "2029",
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
2. `GET /calibration/leaderboard` — check the V10 (Physics & Materials) accuracy row.
3. `GET /v1/substrate/completeness` — V10 substrate-completeness attestation.

## Guidance for agents

- Battery-chemistry forecasts pair with V1 (Energy) and V6 (Supply-Chain); see the
  `cross_product` skill for those crossings.
- Discovery forecasts carry wide bands; pass the full band and assumptions downstream.
