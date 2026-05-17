---
name: phronesis-quantum-forecasting
description: Request, read, and verify quantum-computing forecasts from the Phronesis platform — quantum-supremacy milestones, qubit-scaling trajectories, post-quantum-crypto migration, quantum-chemistry simulation, and quantum-vendor canonical attestation. Enforces a US-and-allied vendor-citation governance rule. Use this skill when an agent needs a structured, uncertainty-banded, audit-verifiable forecast about quantum computing.
---

# Phronesis — Quantum Computing Forecasting (V12)

Phronesis V12 (Quantum Computing) forecasts the quantum-computing trajectory:
quantum-supremacy milestones, qubit-scaling, post-quantum-cryptography migration,
quantum-chemistry simulation capability, and canonical attestation of quantum
vendors. Use this skill when an agent must ground a quantum-technology or
crypto-migration decision in a calibrated forecast.

- **Base URL:** `https://phronesis-jrstinehour.replit.app`
- **Vertical:** V12 — Quantum Computing — Pythia ring `Pythia-Quantum`, Themis cluster `themis-V12-quantum-#101`
- **Transports:** co-equal MCP and REST.

## Vendor-citation governance — MANDATORY (Lesson #28)

V12 forecasts are governed by a vendor-citation rule enforced by the Themis #101.5
rule cluster. **China-origin quantum-vendor citations are prohibited.** This
specifically excludes — but is not limited to:

- **Origin Quantum**
- **SpinQ**
- **Beijing Academy of Quantum Information Sciences**
- **USTC** (University of Science and Technology of China)

V12 forecasts **must cite US-and-allied quantum vendors** (for example IBM Quantum,
Google Quantum AI, Quantinuum, IonQ, Rigetti, PsiQuantum, Atom Computing, and
allied-nation vendors). This is a hard governance rule (Lesson #28). If an agent
supplies a China-origin vendor as a source or anchor in a request, the Themis #101.5
rule rejects the forecast at the publish gate — it is not silently scrubbed. Do not
attempt to work around it.

## Archetypes available

- `quantum-supremacy-milestone` — likelihood / timing of a quantum-supremacy milestone.
- `qubit-scaling-trajectory` — projected qubit-count / fidelity scaling.
- `post-quantum-crypto-migration` — pace of post-quantum-cryptography migration.
- `quantum-chemistry-simulation` — quantum-chemistry simulation capability progression.
- `quantum-vendor-canonical-attestation` — canonical attestation of a quantum vendor.

Confirm via `GET /v1/catalog` (V12 entry's `archetypes_available` + `status`).

## Requesting a forecast

```
POST /v1/decision/forecast
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "vertical": "V12",
  "archetype": "qubit-scaling-trajectory",
  "subject": "quantum",
  "question": "Logical-qubit count achievable by leading US-and-allied vendors by 2028",
  "horizon": "2028",
  "compute_tier": "deep"
}
```

`compute_tier`: `standard`, `deep`, `strategic`, or `strategic-sync`.

## Reading the Decision API output

Contract-v1 envelope `{ "status": "ok", "data": {...}, "request_id": "..." }`. The
`data` decision forecast contains a **point forecast** (`p50`), a monotonic
**uncertainty band** (`p10 / p50 / p90`), enumerated **assumptions**, the **sources**
citation chain (US-and-allied vendors only), **sensitivity** drivers referencing
assumption ids, the **cost attestation**, and an **`audit_trail_id`**.

## Verifying a forecast

1. `GET /v1/trust/receipt/{forecast_id}` — public signed Trust Receipt. The receipt's
   source chain reflects the US-and-allied vendor-citation governance.
2. `GET /calibration/leaderboard` — check the V12 (Quantum) accuracy row.
3. `GET /v1/substrate/completeness` — V12 substrate-completeness attestation.

## Guidance for agents

- Before citing any quantum vendor as a source, confirm it is US-and-allied per the
  Lesson #28 governance rule above.
- Post-quantum-crypto-migration questions pair with V5 (Regulatory); see the
  `cross_product` skill.
- Pass the full uncertainty band downstream and cite the Trust Receipt id.
