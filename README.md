# Agent Delivery Engineering (ADE) Failure Registry

> 跨架构 AI Agent 失效案例登记库 — 由社区驱动，持续演进。

[![Cases](https://img.shields.io/badge/cases-19-brightgreen)](#)
[![Architectures](https://img.shields.io/badge/architectures-3-blue)](./ARCHITECTURES.md)
[![Taxonomy](https://img.shields.io/badge/taxonomy-5%20layers%20×%2015%20codes-orange)](./TAXONOMY.md)

## What is this?

Agent Delivery Engineering (ADE) Failure Registry is a **cross-architecture AI agent failure case knowledge base**, organized by the ADE (Agent Delivery Engineering) five-layer taxonomy.

It's not a bug tracker — it's a **structured encyclopedia of failure patterns** that helps:
- **Researchers**: validate and enrich ADE theory with real-world cases
- **Engineers**: avoid known failure modes when designing new systems
- **Community**: continuously collect and classify failures across architectures

## Five-Layer Taxonomy

```
L1 — Channel Layer        → arXiv:2606.04896
  A: Sender Fracture | B: Receiver Fracture | C: State Sync Fracture

L2 — Observability Layer  → arXiv:2606.08162
  A: Silent Error | B: Success Illusion | C: Recovery Mechanism Silent Failure

L3 — Data Layer           → arXiv:2606.18065
  A: Data Loss | B: Data Decay | C: Data Inconsistency

L4 — Cognitive Layer      → ADE Framework Theory
  A: Knowledge Discontinuity | B: Cognitive Framework Lag | C: Social Norm Deficiency

L5 — Behavior Layer       → ADE Framework Theory
  A: Behavior Violation | B: Behavior Omission | C: Behavior Inconsistency
```

See [TAXONOMY.md](./TAXONOMY.md) for full details.

## Case Index

| ID | Layer | Arch | Severity | Description |
|----|-------|------|----------|-------------|
| [L1A-001](./L1-channel/L1A-001-cron-memory-channel-fracture.md) | L1-A | hermes | Critical | Cron agent memory write channel fracture |
| [L1A-002](./L1-channel/L1A-002-crewai-hierarchical-delegation-broken.md) | L1-A | **crewai** | Critical | Manager cannot delegate to workers |
| [L1B-001](./L1-channel/L1B-001-qqbot-websocket-zombie-18h.md) | L1-B | hermes | Critical | QQ Bot zombie WebSocket — 18h silent death |
| [L1B-002](./L1-channel/L1B-002-wecom-846609-path-state-disconnect.md) | L1-B | hermes | Critical | WeCom 846609: send path never reconnects |
| [L1C-001](./L1-channel/L1C-001-wecom-ack-loss-duplicate-messages.md) | L1-C | hermes | High | WeCom ACK loss → duplicate messages |
| [L2A-001](./L2-observability/L2C-001-cron-approval-deadlock.md) | L2-C | hermes | High | Cron approval deadlock |
| [L2A-003](./L2-observability/L2A-003-crewai-66-silent-exception-swallows.md) | L2-A | **crewai** | Critical | 66 silent exception swallows in core |
| [L2B-002](./L2-observability/L2B-002-crewai-hitl-pre-review-fails-open.md) | L2-B | **crewai** | Critical | HITL safeguard silently bypassed |
| [L2C-002](./L2-observability/L2C-002-wecom-websocket-silent-reconnect.md) | L2-C | hermes | High | WeCom silent reconnect failure |
| [L3A-001](./L3-data/L3A-001-context-compression-data-loss.md) | L3-A | hermes | Critical | Context compression loses unflushed messages |
| [L3A-002](./L3-data/L3A-002-v4a-patch-9bugs-data-loss.md) | L3-A | hermes | Critical | V4A patch parser: 9 data loss bugs |
| [L3A-003](./L3-data/L3A-003-crewai-bedrock-tool-args-silently-dropped.md) | L3-A | **crewai** | High | Bedrock tool arguments silently dropped |
| [L3B-001](./L3-data/L3B-001-state-db-fts-corruption.md) | L3-B | hermes | Critical | FTS corruption undetected |
| [L3C-001](./L3-data/L3C-001-desktop-delete-profile-state-inconsistency.md) | L3-C | hermes | High | Desktop state inconsistency |
| [L3C-002](./L3-data/L3C-002-autogen-graphflow-state-corruption.md) | L3-C | **autogen** | Critical | GraphFlow state corruption after interruption |
| [L4B-001](./L4-cognitive/L4B-001-crewai-behavioral-drift-across-sessions.md) | L4-B | **crewai** | High | Silent behavioral drift across sessions |
| [L5A-001](./L5-behavior/L5A-001-email-auto-reply.md) | L5-A | hermes | Critical | Email auto-reply without authorization |
| [L5A-002](./L5-behavior/L5A-002-crewai-tool-retry-no-idempotency.md) | L5-A | **crewai** | Critical | Tool retry: no idempotency, duplicate payments |
| [L5A-003](./L5-behavior/L5A-003-autogen-guardrails-fail-106k-loss.md) | L5-A | **autogen** | Critical | All guardrails fail — $106K loss |

### Statistics

| Layer | Count | Critical | High |
|-------|-------|----------|------|
| L1 Channel | 5 | 4 | 1 |
| L2 Observability | 4 | 2 | 2 |
| L3 Data | 6 | 5 | 1 |
| L4 Cognitive | 1 | 0 | 1 |
| L5 Behavior | 3 | 3 | 0 |
| **Total** | **19** | **14** | **5** |

### Architecture Coverage

| Architecture | Cases | Key Patterns |
|-------------|-------|-------------|
| **Hermes Agent** | 11 | WebSocket half-open, cron context degradation, cross-platform state drift |
| **CrewAI** | 6 | Silent exception swallowing, behavioral drift, delegation fracture |
| **AutoGen** | 2 | State corruption on interruption, guardrail bypass ($106K) |
| LangChain | — | Coming soon |
| OpenAI Assistants | — | Coming soon |
| Claude | — | Coming soon |

### Cross-Architecture Patterns

These patterns appear in **multiple architectures** — proving they are universal, not framework-specific:

| Pattern | Hermes | CrewAI | AutoGen |
|---------|--------|--------|---------|
| Silent exception swallowing | ✓ | ✓ (66 instances!) | — |
| Cross-session knowledge loss | ✓ | ✓ | — |
| Context compression data loss | ✓ | ✓ | — |
| Duplicate delivery (no dedup) | ✓ | ✓ | — |
| Safety mechanism bypass | ✓ | ✓ | ✓ |
| State inconsistency after interruption | ✓ | — | ✓ |

## How to Contribute

1. Read [CONTRIBUTING.md](./CONTRIBUTING.md)
2. Use [TEMPLATE.md](./TEMPLATE.md)
3. Submit a PR

## About ADE

**Agent Delivery Engineering (ADE)** — Stability Forces Engineering. The engineering movement of humans against intelligence entropy.

## License

[CC-BY-4.0](./LICENSE)

---

*Last updated: 2026-06-17 | Maintained by Dexing Liu, Qijing Digital Technology*
