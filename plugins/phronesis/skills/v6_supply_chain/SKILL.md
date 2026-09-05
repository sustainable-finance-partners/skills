---
name: phronesis-supply-chain-forecasting
description: Request, read, and verify supply-chain forecasts from the Phronesis platform — critical minerals, agriculture supply, and freight / port congestion. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about supply-chain dynamics rather than a free-text guess.
---

# Phronesis — Supply-Chain Forecasting (V6)

Phronesis V6 (Supply-Chain) forecasts the physical supply chains underpinning the
energy transition and the broader economy: critical-mineral availability,
agricultural supply, and freight and port congestion. Use this skill when an agent
must ground a sourcing, logistics, or inventory decision in a calibrated forecast.

- **Base URL:** `https://api.phronesisintel.com`
- **Vertical:** V6 — Supply-Chain — Pythia ring `Pythia-SupplyChain`, Themis cluster `themis-V6-supplychain`
- **Transports:** semantic parity across MCP and REST; transport-appropriate projection.

## Archetypes available

- `critical-minerals` — supply / price trajectory for lithium, cobalt, copper, etc.
- `agriculture-supply` — agricultural-commodity supply projections.
- `freight-port-congestion` — freight throughput and port-congestion forecasts.

Confirm via `GET /v1/catalog` (V6 entry's `archetypes_available` + `status`).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V6",
  "archetype": "critical-minerals",
  "subject": "supply_chain",
  "question": "Lithium carbonate supply-demand balance through 2028",
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
2. `GET /calibration/leaderboard` — check the V6 (Supply-Chain) accuracy row.
3. `GET /v1/substrate/completeness` — V6 substrate-completeness attestation.

## Guidance for agents

- Critical-minerals forecasts pair naturally with V1 (Energy) renewable-deployment
  and V8 (Robotics) — consider the `cross_product` skill for those crossings.
- Pass the full band downstream and cite the Trust Receipt id.

## Assurance trail (`.phronesis/`)

When this skill guides a real decision task, keep a per-task working directory
`.phronesis/<task-slug>/` in the calling agent's workspace and maintain the six-file
assurance trail as the task progresses:

1. `decision_requirement.md` — the decision in one sentence, owner, materiality, horizon (written first).
2. `evidence_notes.md` — evidence consulted, with sources and timestamps.
3. `action_boundary_request.json` — the exact boundary/assessment request sent, verbatim.
4. `decision_receipt.json` — the receipt/forecast output returned, verbatim — never edited.
5. `outcome_followup.md` — what was decided and the trigger that revisits it.
6. `review_log.md` — dated review notes, append-only.

Full convention: `docs/DOT_PHRONESIS_CONVENTION.md` in this repository.
