# Phronesis Skills

Agent Skills for **Phronesis** — an energy-transition AI forecasting platform built by
Sustainable Finance Partners.

## What Phronesis is

Phronesis is a future-state intelligence engine for the energy transition and its
adjacent frontier domains. Specialist AI agents ingest research, energy and climate
data, policy, and customer context, then produce **decision forecasts** that overlay
emerging technology trajectories on current business models. Phronesis sells
primarily via API/MCP to AI agents, and secondarily via a thin SaaS UI to human
capital allocators.

Every Phronesis forecast is delivered through a structured **Decision API** output:
a point forecast, a `p10 / p50 / p90` uncertainty band, explicit assumptions, cited
sources, and a per-call cost attestation. Forecasts are individually verifiable
through signed **Trust Receipts**, and per-vertical accuracy is published openly on
the **Calibration Leaderboard**.

Phronesis exposes **co-equal MCP and REST transports** — neither is the "real" API;
both pass through the same governance boundary. The production base URL is:

```
https://phronesis-jrstinehour.replit.app
```

## What this repo provides

This repository contains **Claude Code Agent Skills** that teach an agent how to
actually use Phronesis: which archetypes each vertical supports, how to call the
forecast endpoint, how to read the structured Decision API output, and how to verify
a forecast after the fact. There is one skill per vertical, plus a cross-vertical
skill for forecasting at the intersection of two verticals.

Each skill is a `SKILL.md` file with YAML frontmatter (`name`, `description`) and a
markdown instruction body, located at
`plugins/phronesis/skills/<skill-name>/SKILL.md`.

## Skills

| Skill | Directory | Scope |
|---|---|---|
| `phronesis-energy-forecasting` | `v1_energy` | Energy forecasting — grid resilience, demand-load, capacity planning, renewable deployment |
| `phronesis-compute-forecasting` | `v2_compute` | Compute & AI-infrastructure capacity forecasting — chips, datacenters, training/inference demand |
| `phronesis-healthcare-forecasting` | `v3_hygieia` | Healthcare forecasting — drug pipeline, trials, policy; HIPAA-Safe-Harbor / population-aggregate only |
| `phronesis-climate-forecasting` | `v4_climate` | Climate physical-risk forecasting for insurance and reinsurance |
| `phronesis-regulatory-forecasting` | `v5_regulatory` | Cross-cutting regulatory-compliance and policy-shift forecasting |
| `phronesis-supply-chain-forecasting` | `v6_supply_chain` | Supply-chain forecasting — critical minerals, freight, agriculture |
| `phronesis-space-forecasting` | `v7_space` | Space & space-economy forecasting — launch cadence, constellations, monitoring |
| `phronesis-robotics-forecasting` | `v8_robotics` | Robotics safety & deployment forecasting |
| `phronesis-ai-agi-forecasting` | `v9_ai_agi` | AI/AGI capability & safety forecasting |
| `phronesis-physics-materials-forecasting` | `v10_physics_materials` | Physics & materials-discovery forecasting |
| `phronesis-longevity-forecasting` | `v11_longevity` | Longevity & human-health forecasting |
| `phronesis-quantum-forecasting` | `v12_quantum` | Quantum-computing forecasting (US-and-allied vendor citation governance) |
| `phronesis-cross-product-forecasting` | `cross_product` | Cross-vertical forecasting at V#X x V#Y intersections |

## Distribution

### Claude Code

Add this repository as a plugin marketplace, then install the `phronesis` plugin:

```
/plugin marketplace add sustainable-finance-partners/skills
/plugin install phronesis@phronesis-skills
```

### Vercel Skills CLI

```
npx skills add sustainable-finance-partners/skills
```

## Phronesis MCP server

These skills are companions to the live Phronesis service. The agent-discoverable
entry point is:

- MCP / REST base URL: `https://phronesis-jrstinehour.replit.app`
- Agent discovery: `GET /.well-known/agent-discovery.json`
- Vertical + archetype catalog: `GET /v1/catalog`
- Pricing tiers: `GET /v1/pricing`
- Decision forecast (JWT-gated): `POST /v1/decision/forecast`
- Substrate completeness: `GET /v1/substrate/completeness`
- Trust Receipt verification: `GET /v1/trust/receipt/{forecast_id}`
- Calibration Leaderboard: `GET /calibration/leaderboard`

All `/.well-known/*.json` surfaces and the catalog/pricing/leaderboard endpoints are
public-by-design. The forecast endpoint requires a JWT.

## License

Apache License 2.0 — see [`LICENSE`](./LICENSE).
