---
name: phronesis-longevity-forecasting
description: Request, read, and verify longevity and human-health forecasts from the Phronesis platform — lifespan-extension trajectories, biomarker-clock validation, geroscience-trial outcomes, longevity-therapy pipelines, and cellular-rejuvenation substrate. Operates on population-aggregate data only. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about longevity science.
---

# Phronesis — Longevity Forecasting (V11)

Phronesis V11 (Longevity & Human Health) forecasts the science of human longevity:
lifespan-extension trajectories, biomarker-clock validation, geroscience-trial
outcomes, longevity-therapy pipelines, and the maturation of cellular-rejuvenation
substrate. Use this skill when an agent must ground a research or
longevity-investment decision in a calibrated forecast.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V11 — Longevity & Human Health — Pythia ring `Pythia-Longevity`, Themis cluster `themis-V11-longevity-#99`
- **Transports:** co-equal MCP and REST.

## Data discipline — read first

V11 forecasts on **population-aggregate, cohort-level data only**. Do not send
individual-identifiable health data in a request; subject matter must be
population-, cohort-, or trial-level. Output is decision-support, not medical advice.

## Archetypes available

- `lifespan-extension-trajectory` — projected pace of lifespan / healthspan extension.
- `biomarker-clock-validation` — likelihood a biomarker / aging clock is validated.
- `geroscience-trial-outcome` — probability a geroscience trial meets its endpoint.
- `longevity-therapy-pipeline` — pipeline-stage progression for longevity therapies.
- `cellular-rejuvenation-substrate` — maturation of cellular-rejuvenation technology.

Confirm via `GET /v1/catalog` (V11 entry's `archetypes_available` + `status`).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V11",
  "archetype": "geroscience-trial-outcome",
  "subject": "longevity",
  "question": "Probability a major rapamycin healthspan trial meets its primary endpoint by 2028",
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
2. `GET /calibration/leaderboard` — check the V11 (Longevity) accuracy row.
3. `GET /v1/substrate/completeness` — V11 substrate-completeness attestation.

## Guidance for agents

- Keep all inputs population-aggregate; never attempt individual-level prediction.
- Present probability forecasts with the full band and assumptions.
- Treat output as decision-support; do not relay it as clinical guidance.
