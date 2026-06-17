# L1A-002: CrewAI Hierarchical Delegation — Manager Cannot Reach Workers

## Case ID
L1A-002

## Date
2026-04-17

## Source
- **Repository**: [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI)
- **Issue**: [#4783 - Hierarchical process delegation fails](https://github.com/crewAIInc/crewAI/issues/4783)
- **Architecture**: crewai

## Classification
- **Layer**: L1-A (Channel Layer — Sender-side Fracture)
- **Severity**: Critical
- **Impact**: Hierarchical process behaves like sequential — core multi-agent coordination broken

## Description

### What Happened
In CrewAI's hierarchical process, manager agents are unable to delegate tasks to worker agents even when `allow_delegation=True`. The manager only executes tasks using its own tools and never delegates to other agents.

The hierarchical process — the core multi-agent coordination mechanism — silently degrades to sequential execution.

### Root Cause
The delegation channel between manager and workers is fractured. The manager's send path (delegation logic) cannot reach the workers' receive path (task acceptance). The hierarchical process falls back to sequential without any error or warning.

### Evidence
- Reproduction: Create Crew with `process=Process.hierarchical` → manager executes alone
- Expected: manager delegates subtasks to workers → Actual: manager does everything itself

## Connection to ADE Theory

### Which ADE Principle Does This Violate?
**Sender-side Fracture (L1-A)**: The manager (sender) attempts to delegate but the delegation channel to workers (receivers) is broken. The manager believes it's coordinating, but workers never receive tasks.

**Silent Failure (L2)**: The hierarchical process silently degrades to sequential — no error, no warning, no indication that the core feature is broken.

### How Could ADE Prevent This?
1. **Delegation Verification**: Confirm worker received delegated task before proceeding
2. **Process Mode Health Check**: Verify hierarchical delegation actually occurred
3. **Fallback Detection**: Alert when hierarchical process falls back to sequential

## Fix / Defense
- Fix delegation channel to properly route tasks to workers
- Add delegation success verification
- Alert when hierarchical process degrades to sequential

## Related Cases
- L1A-001 (Hermes cron memory fracture) — same pattern: cross-agent channel silently broken
- L1B-002 (Hermes WeCom 846609) — send path knows it's broken but doesn't trigger recovery

## References
- GitHub Issue: [#4783](https://github.com/crewAIInc/crewAI/issues/4783)
- ADE Paper: [Channel Fracture](https://arxiv.org/abs/2606.04896)

---

## Metadata
- **Discovered by**: Community report
- **Reviewed by**: Dexing Liu
- **Last updated**: 2026-06-17
