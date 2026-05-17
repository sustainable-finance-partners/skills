---
name: phronesis-climate-forecasting
description: Request, read, and verify climate physical-risk forecasts from the Phronesis platform — physical-risk attestation, extreme-event probability, and climate-scenario projection for insurance and reinsurance. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable climate-risk forecast rather than a free-text guess.
---

# Phronesis — Climate Forecasting (V4)

Phronesis V4 (Climate) forecasts climate physical risk for insurance, reinsurance,
and asset-exposure decisions: the probability and severity of extreme events and the
trajectory of climate scenarios. Use this skill when an agent must price, reserve
against, or underwrite physical climate risk.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V4 — Climate — Pythia ring `Pythia-Climate`, Themis cluster `themis-V4-climate`
- **Transports:** co-equal MCP and REST.

## Archetypes available

- `physical-risk-attestation` — attested physical-risk exposure for an asset / region.
- `extreme-event-probability` — probability of a defined extreme event in a horizon.
- `climate-scenario-projection` — projection under a named climate scenario.

Confirm via `GET /v1/catalog` (V4 entry's `archetypes_available` + `status`).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V4",
  "archetype": "extreme-event-probability",
  "subject": "climate",
  "question": "Probability of a Category 4+ hurricane landfall in the Gulf Coast in the 2027 season",
  "horizon": "2027",
  "region": "US-Gulf-Coast",
  "compute_tier": "strategic"
}
```

`compute_tier`: `standard`, `deep`, `strategic`, or `strategic-sync`. Physical-risk
underwriting decisions usually warrant `strategic`.

## Reading the Decision API output

Contract-v1 envelope `{ "status": "ok", "data": {...}, "request_id": "..." }`. The
`data` decision forecast contains: a **point forecast** (`p50`), a monotonic
**uncertainty band** (`p10 / p50 / p90`), enumerated **assumptions**, the **sources**
citation chain, **sensitivity** drivers referencing assumption ids, the **cost
attestation**, and an **`audit_trail_id`**.

For climate, the band width is itself decision-relevant: a wide `p10`-to-`p90` spread
is a signal about model and scenario uncertainty, not noise to discard.

## Verifying a forecast

1. `GET /v1/trust/receipt/{forecast_id}` — public signed Trust Receipt.
2. `GET /calibration/leaderboard` — check the V4 (Climate) accuracy row vs baselines.
3. `GET /v1/substrate/completeness` — V4 substrate-completeness attestation.

## Guidance for agents

- Always carry the full uncertainty band into pricing / reserving logic.
- Name the climate scenario explicitly in the request for scenario-projection work.
- Cite the Trust Receipt `audit_trail_id` so an underwriting decision stays auditable.
