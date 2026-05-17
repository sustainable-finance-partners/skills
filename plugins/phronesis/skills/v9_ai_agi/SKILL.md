---
name: phronesis-ai-agi-forecasting
description: Request, read, and verify AI/AGI capability and safety forecasts from the Phronesis platform — AGI timelines, capability deltas, alignment-research milestones, AI-safety regulatory cadence, and compute-capability economics. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about AI progress rather than a free-text guess.
---

# Phronesis — AI/AGI Forecasting (V9)

Phronesis V9 (AI/AGI & Compute) forecasts artificial-intelligence capability and
safety: AGI-timeline projections, capability-delta substrate between model
generations, alignment-research milestones, AI-safety regulatory cadence, and the
economics of compute-driven capability. Use this skill when an agent must ground a
strategic decision about AI progress in a calibrated forecast.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V9 — AI/AGI & Compute — Pythia ring `Pythia-AI-AGI`, Themis cluster `themis-V9-ai-agi-#95`
- **Transports:** co-equal MCP and REST.

## Archetypes available

- `agi-timeline-forecast` — projected timing of defined AGI milestones.
- `capability-delta-substrate` — capability gap / delta between model generations.
- `alignment-research-milestone` — likelihood / timing of alignment-research milestones.
- `ai-safety-regulatory-cadence` — pace of AI-safety regulation.
- `compute-capability-economics` — economics linking compute spend to capability gain.

Confirm via `GET /v1/catalog` (V9 entry's `archetypes_available` + `status`).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V9",
  "archetype": "agi-timeline-forecast",
  "subject": "ai_agi",
  "question": "Probability a frontier model meets the defined AGI capability bar before 2030",
  "horizon": "2030",
  "compute_tier": "strategic"
}
```

`compute_tier`: `standard`, `deep`, `strategic`, or `strategic-sync`. Long-horizon
AGI questions usually warrant `strategic`.

## Reading the Decision API output

Contract-v1 envelope `{ "status": "ok", "data": {...}, "request_id": "..." }`. The
`data` decision forecast contains a **point forecast** (`p50`), a monotonic
**uncertainty band** (`p10 / p50 / p90`), enumerated **assumptions**, the **sources**
citation chain, **sensitivity** drivers referencing assumption ids, the **cost
attestation**, and an **`audit_trail_id`**. For AGI-timeline work the band width is a
first-class signal — long-horizon forecasts carry wide, honest bands.

## Verifying a forecast

1. `GET /v1/trust/receipt/{forecast_id}` — public signed Trust Receipt.
2. `GET /calibration/leaderboard` — check the V9 (AI/AGI) accuracy row vs baselines.
3. `GET /v1/substrate/completeness` — V9 substrate-completeness attestation.

## Guidance for agents

- AI/AGI questions are frequently best framed as intersections — AI/AGI x Regulatory,
  AI/AGI x Compute, AI/AGI x Robotics. See the `cross_product` skill.
- Always carry the full band and the assumptions downstream; never collapse a
  long-horizon AGI forecast to a single point.
