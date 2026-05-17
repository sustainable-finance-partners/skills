---
name: phronesis-energy-forecasting
description: Request, read, and verify energy-transition forecasts from the Phronesis platform — grid resilience, demand-load, capacity planning, renewable deployment, and carbon intensity. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about energy systems rather than a free-text guess.
---

# Phronesis — Energy Forecasting (V1)

Phronesis V1 (Energy) forecasts the energy transition: load growth, renewable
deployment trajectories, grid stress, and carbon intensity. Use this skill when an
agent must ground an energy-related decision in a calibrated forecast with an
uncertainty band, explicit assumptions, cited sources, and an audit trail — not a
free-text estimate.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V1 — Energy — Pythia ring `Pythia-Energy`, Themis cluster `themis-V1-energy`
- **Transports:** co-equal MCP and REST. Either reaches the same governance boundary.

## Archetypes available

Pick the archetype that matches the decision before calling the endpoint:

- `demand-load-forecast` — electricity demand / load growth over a horizon.
- `renewable-deployment` — solar / wind / storage build-out trajectories.
- `grid-resilience` — grid stress, congestion, and reliability under scenarios.
- `carbon-intensity` — grid carbon-intensity trajectory for a region.

Confirm the archetype is live by calling `GET /v1/catalog` and checking the V1
entry's `archetypes_available` list and `status: LIVE`.

## Requesting a forecast

Call the JWT-gated Decision API:

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V1",
  "archetype": "demand-load-forecast",
  "subject": "energy",
  "question": "Peak summer load for the ERCOT grid region in 2028",
  "horizon": "2028",
  "region": "ERCOT",
  "compute_tier": "deep"
}
```

`compute_tier` is abstract — one of `standard`, `deep`, `strategic`, or
`strategic-sync`. Deeper tiers buy more reasoning depth and source breadth; the
per-call cost is attested in the response, not guessed by the caller.

## Reading the Decision API output

The response is a contract-v1 envelope: `{ "status": "ok", "data": { ... },
"request_id": "..." }`. The `data` object is the canonical decision forecast:

- **Point forecast** — the headline `p50` value with units.
- **Uncertainty band** — `p10` / `p50` / `p90`. The band is monotonic
  (`p10 <= p50 <= p90`); always present this band, never the point alone.
- **Assumptions** — an enumerated list of named assumptions, each with an id.
- **Sources** — the citation chain backing the forecast.
- **Sensitivity** — drivers that move the forecast, each referencing an assumption id.
- **Cost attestation** — the attested per-call cost (Phronesis Sacred Invariant #7).
- **`audit_trail_id`** — the id used to fetch the Trust Receipt later.

## Verifying a forecast

1. **Trust Receipt** — `GET /v1/trust/receipt/{forecast_id}` returns a public,
   signed receipt: the scrubbed forecast, its methodology attestation, and
   timestamps. Model identity and algorithm internals are intentionally stripped.
2. **Calibration Leaderboard** — `GET /calibration/leaderboard` shows per-vertical
   accuracy (MAPE, RMSE, pinball, Brier) versus naive-LLM and other baselines. Check
   the V1 (Energy) row for empirical track record before relying on a forecast.
3. **Substrate completeness** — `GET /v1/substrate/completeness` attests how complete
   the V1 data substrate is.

## Guidance for agents

- Always pass the forecast's full uncertainty band downstream, not just `p50`.
- Surface the assumptions and sources to the human or agent consuming the decision.
- For high-stakes capacity-planning decisions, prefer `strategic` compute tier and
  cite the Trust Receipt `audit_trail_id` in your own output so the decision chain
  stays verifiable.
