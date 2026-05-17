---
name: phronesis-cross-product-forecasting
description: Request, read, and verify cross-vertical forecasts from the Phronesis platform — decisions that live at the intersection of two verticals (e.g. AI/AGI x Regulatory, Energy x Compute, Climate x Supply-Chain). Use this skill when a decision depends on how two domains interact rather than on either alone.
---

# Phronesis — Cross-Product Forecasting (V#X x V#Y)

Many real decisions live at the intersection of two Phronesis verticals: an
AI/AGI capability trajectory conditioned on regulatory cadence, datacenter build-out
conditioned on grid capacity, or critical-mineral supply conditioned on climate
risk. Phronesis V#X x V#Y cross-product forecasting models that interaction
directly. Use this skill when a decision depends on how two domains move together —
not on either domain in isolation.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Transports:** co-equal MCP and REST.

## The twelve verticals

`V1` Energy, `V2` Compute + AI Infrastructure, `V3` Healthcare, `V4` Climate,
`V5` Regulatory, `V6` Supply-Chain, `V7` Space, `V8` Robotics, `V9` AI/AGI & Compute,
`V10` Physics & Materials Discovery, `V11` Longevity & Human Health, `V12` Quantum
Computing. Per-vertical archetypes and live status are listed at `GET /v1/catalog`.

## Common intersections

- **V9 x V5** — AI/AGI capability conditioned on AI-safety regulatory cadence.
- **V2 x V1** — datacenter / compute build-out conditioned on grid and power supply.
- **V4 x V6** — supply-chain disruption conditioned on climate physical risk.
- **V1 x V5** — renewable deployment conditioned on RPS / IRA policy.
- **V8 x V5** — robotics labor displacement conditioned on regulation.
- **V10 x V1** — battery-chemistry breakthroughs conditioned on energy demand.
- **V12 x V5** — post-quantum-crypto migration conditioned on regulatory mandates.

## Requesting a cross-product forecast

Name both verticals; pick the archetype from whichever vertical is the primary
forecast target, and pass the second vertical as the conditioning context:

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V9",
  "cross_vertical": "V5",
  "archetype": "agi-timeline-forecast",
  "subject": "ai_agi",
  "question": "Probability a frontier model meets the AGI capability bar before 2030, conditioned on the pace of AI-safety regulation",
  "horizon": "2030",
  "compute_tier": "strategic"
}
```

Cross-product questions are inherently higher-variance; `strategic` or
`strategic-sync` compute tiers are usually warranted.

## Reading the Decision API output

Contract-v1 envelope `{ "status": "ok", "data": {...}, "request_id": "..." }`. The
`data` decision forecast contains a **point forecast** (`p50`), a monotonic
**uncertainty band** (`p10 / p50 / p90`), enumerated **assumptions** — for a
cross-product forecast the assumptions name the linkage between the two verticals —
the **sources** citation chain, **sensitivity** drivers referencing assumption ids,
the **cost attestation**, and an **`audit_trail_id`**.

When either vertical in the crossing is governed (e.g. V3 / V11 population-aggregate
discipline, V12 US-and-allied vendor-citation rule), that vertical's governance still
applies to the cross-product forecast.

## Verifying a forecast

1. `GET /v1/trust/receipt/{forecast_id}` — public signed Trust Receipt.
2. `GET /calibration/leaderboard` — check the accuracy rows for both verticals.
3. `GET /v1/substrate/completeness` — substrate-completeness for both verticals.

## Guidance for agents

- State the conditioning relationship explicitly in the `question` — what depends on
  what — so the forecast models the right direction of influence.
- The cross-product band is typically wider than either single-vertical band; carry
  the full band and the linkage assumptions downstream.
- Cite the Trust Receipt `audit_trail_id` so the cross-domain decision stays auditable.
