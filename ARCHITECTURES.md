# Architecture Index — 架构索引

> Agent Delivery Engineering (ADE) Failure Registry covers multiple AI Agent architectures. Each architecture's failures are analyzed using the unified ADE five-layer taxonomy.

## Why Cross-Architecture?

ADE failure taxonomy is based on the **essential mechanism** of failures, not bound to any specific framework. The same classification (e.g., L1-B Receiver Fracture) manifests differently across architectures, but the root cause is identical.

Cross-architecture case collection proves:
1. **Theory completeness**: if the taxonomy covers failures across all architectures, it's complete
2. **Universality**: ADE principles are not framework-specific
3. **Community value**: others validate our theory by reporting matching failures
4. **Pattern recognition**: cross-architecture patterns reveal universal laws

## Covered Architectures

### Hermes Agent (11 cases)

**Overview**: Personal AI agent framework by NousResearch. Multi-platform messaging (WeChat, WeCom, Feishu, Telegram, Discord). Plugin-based extensibility. SQLite state management.

**Key Patterns**:
- WebSocket half-open connections (zombie for 18h, silent reconnect failure)
- Cron agent context degradation across long sessions
- Cross-platform state drift (desktop vs server)
- Email Gateway auto-reply without authorization
- V4A patch parser with 9 data loss bugs
- FTS index corruption undetected for 35 days

**Case Distribution**: L1:4, L2:2, L3:4, L5:1

### CrewAI (6 cases)

**Overview**: Multi-agent orchestration framework. Hierarchical/sequential process modes. Task delegation between agents. Memory and learning systems.

**Key Patterns**:
- **66 silent exception swallows** across core modules (memory, reasoning, tools, CLI)
- Silent behavioral drift across session boundaries (ghost lexicon, tool sequence shift)
- Tool retry without idempotency guard (duplicate payments/emails/trades)
- HITL pre-review fails open (safeguard silently bypassed)
- Hierarchical delegation channel fracture (manager can't reach workers)
- Bedrock tool arguments silently dropped (data loss in parsing layer)

**Case Distribution**: L1:1, L2:2, L3:1, L4:1, L5:1

### AutoGen (2 cases)

**Overview**: Multi-agent conversation framework by Microsoft. GraphFlow workflow system. Supports distributed agent runtime.

**Key Patterns**:
- GraphFlow state corruption after interruption (workflow permanently stuck)
- **All guardrails fail** — 56-day proof, $106,000+ business loss, AWS account destroyed

**Case Distribution**: L3:1, L5:1

---

## Architectures to Cover (Coming Soon)

| Architecture | Status | Focus Areas |
|-------------|--------|-------------|
| **LangChain/LangGraph** | Coming soon | Agent executor failures, graph state persistence, tool parsing |
| **OpenAI Assistants API** | Coming soon | Thread state management, file search failures, function calling |
| **Claude (Anthropic)** | Coming soon | Tool use failures, context window management, conversation state |
| **AutoGPT** | Coming soon | Autonomous task planning failures, memory persistence |

## Cross-Architecture Pattern Matrix

| Pattern | Hermes | CrewAI | AutoGen | Universal? |
|---------|--------|--------|---------|-----------|
| Silent exception swallowing | ✓ | ✓ (66!) | — | ✓ |
| Cross-session knowledge loss | ✓ | ✓ | — | ✓ |
| Context compression data loss | ✓ | ✓ | — | ✓ |
| Duplicate delivery (no dedup) | ✓ | ✓ | — | ✓ |
| Safety mechanism bypass | ✓ | ✓ | ✓ | ✓ |
| State inconsistency after interruption | ✓ | — | ✓ | ✓ |
| Tool argument loss in transit | ✓ | ✓ | — | ✓ |
| Behavioral drift after reset | — | ✓ | ✓ | ✓ |

**Universal patterns** (appear in 2+ architectures) are the strongest evidence for ADE theory's completeness.

## How to Add Your Architecture

If you use a framework not listed above and find failures matching ADE taxonomy:
1. Open an issue with the failure description
2. Use [TEMPLATE.md](./TEMPLATE.md) to document
3. Submit a PR to add to this index

---

*Last updated: 2026-06-17*
