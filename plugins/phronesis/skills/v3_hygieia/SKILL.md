---
name: phronesis-healthcare-forecasting
description: Request, read, and verify healthcare forecasts from the Phronesis Hygieia ring — drug-pipeline progression, biotech-trial outcomes, and healthcare-policy impact. Operates strictly on HIPAA-Safe-Harbor population-aggregate data only. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable healthcare forecast rather than a free-text guess.
---

# Phronesis — Healthcare Forecasting (V3 / Hygieia)

Phronesis V3 (Healthcare) — the **Hygieia** ring — forecasts drug-pipeline
progression, clinical-trial outcomes, and the impact of healthcare policy. Use this
skill when an agent must ground a healthcare or biotech decision in a calibrated
forecast.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V3 — Healthcare — Pythia ring `Hygieia`, Themis cluster `themis-V3-healthcare`
- **Transports:** co-equal MCP and REST.

## HIPAA / PHI discipline — read first

V3 operates on **population-aggregate, HIPAA-Safe-Harbor data only**. It does not
ingest, store, or forecast against individual-level protected health information.

- Never send patient-identifiable data in a forecast request. Subject matter must be
  population-level, cohort-level, trial-level, or policy-level.
- The Themis healthcare cluster enforces this at the publish gate; requests carrying
  PHI-shaped input are rejected, not silently scrubbed.
- Forecasts are decision-support, not medical advice.

## Archetypes available

- `drug-pipeline-progression` — likelihood / timing of pipeline-stage advancement.
- `biotech-trial-outcome` — probability of a clinical-trial endpoint being met.
- `healthcare-policy-impact` — projected effect of a healthcare-policy change.

Confirm via `GET /v1/catalog` (V3 entry).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V3",
  "archetype": "biotech-trial-outcome",
  "subject": "healthcare",
  "question": "Probability a Phase III GLP-1 cardiovascular-outcomes trial meets its primary endpoint by 2027",
  "horizon": "2027",
  "compute_tier": "deep"
}
```

`compute_tier`: `standard`, `deep`, `strategic`, or `strategic-sync`.

## Reading the Decision API output

Contract-v1 envelope `{ "status": "ok", "data": {...}, "request_id": "..." }`. The
`data` decision forecast contains: a **point forecast** (`p50`), a monotonic
**uncertainty band** (`p10 / p50 / p90` — for probability archetypes this is a
probability band), enumerated **assumptions**, the **sources** citation chain,
**sensitivity** drivers referencing assumption ids, the **cost attestation**, and an
**`audit_trail_id`**.

## Verifying a forecast

1. `GET /v1/trust/receipt/{forecast_id}` — public signed Trust Receipt.
2. `GET /calibration/leaderboard` — check the V3 (Healthcare) accuracy row.
3. `GET /v1/substrate/completeness` — V3 substrate-completeness attestation.

## Guidance for agents

- Keep all inputs population-aggregate; never attempt per-patient prediction.
- Present probability forecasts with the full band and the assumptions.
- Treat output as decision-support; do not relay it as clinical guidance.
