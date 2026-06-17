# L5A-002: CrewAI Tool Re-execution on Retry — No Idempotency Guard

## Case ID
L5A-002

## Date
2026-06-13

## Source
- **Repository**: [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI)
- **Issue**: [#5802 - Tool re-execution on task retry has no idempotency guard](https://github.com/crewAIInc/crewAI/issues/5802)
- **Architecture**: crewai

## Classification
- **Layer**: L5-A (Behavior Layer — Behavior Violation)
- **Severity**: Critical
- **Impact**: Duplicate payments, emails, trades — irreversible external side effects

## Description

### What Happened
When a CrewAI task fails and is retried (via max_retry_limit, exception handling, or external re-trigger), any tool that already executed runs again. There's no mechanism to detect that a specific tool call already completed.

Example: `@tool send_payment(amount, recipient)` calls `stripe.charge()` — on retry, the payment fires twice.

### Root Cause
CrewAI's retry mechanism re-executes the entire tool call chain without checking whether a previous execution already produced the intended side effect. In-memory dedup doesn't survive worker re-dispatch.

### Evidence
- Two independent parties confirmed the exact failure mode
- Same pattern documented in [LangGraph Cloud issue #7417](https://github.com/langchain-ai/langgraph/issues/7417)
- Fix requires durable external storage keyed before execution starts

## Connection to ADE Theory

### Which ADE Principle Does This Violate?
**Behavior Violation (L5-A)**: The agent performs an action (payment) that violates the intended behavior contract ("send payment once"). The retry mechanism transforms a single-action intent into a duplicate-action reality.

**BCP (Behavior Contract Protocol)**: The behavior contract specifies "send payment" — the system delivers "send payment twice."

### How Could ADE Prevent This?
1. **Idempotency Guard**: Derive stable request_id from tool arguments, claim in durable storage before execution
2. **BCP Enforcement**: Behavior contracts must specify idempotency requirements for all side-effect tools
3. **Pre-Execution Verification**: Check durable storage before executing any tool with external side effects

## Fix / Defense
- Derive stable request_id from tool arguments before execution
- Claim request_id in durable external storage
- Return cached result on any retry with same ID
- Reference: `safeagent-exec-guard` (pip install)

## Related Cases
- L1C-001 (Hermes WeCom ACK loss duplicate messages) — same anti-pattern: duplicate delivery without dedup

## References
- GitHub Issue: [#5802](https://github.com/crewAIInc/crewAI/issues/5802)
- Cross-reference: [LangGraph #7417](https://github.com/langchain-ai/langgraph/issues/7417)
- ADE Paper: [Channel Fracture](https://arxiv.org/abs/2606.04896)

---

## Metadata
- **Discovered by**: Community report
- **Reviewed by**: Dexing Liu
- **Last updated**: 2026-06-17
