# L4B-001: CrewAI Silent Behavioral Drift Across Session Boundaries

## Case ID
L4B-001

## Date
2026-06-17

## Source
- **Repository**: [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI)
- **Issue**: [#5155 - RFC: Detecting silent behavioral drift in agents across session boundaries](https://github.com/crewAIInc/crewAI/issues/5155)
- **Architecture**: crewai

## Classification
- **Layer**: L4-B (Cognitive Layer — Cognitive Framework Lag)
- **Severity**: High
- **Impact**: Agent behavioral fingerprint silently shifts after context rotation, crew-level output degrades without attributable task failure

## Description

### What Happened
CrewAI crews running multi-step tasks across sessions silently change behavior after context compression or memory rotation — without triggering exceptions or failed tasks. The agent completes the work, but its behavioral fingerprint has shifted.

Three measurable signals indicate drift:
1. **Ghost lexicon decay** — domain vocabulary that appeared reliably disappears after session boundary
2. **Tool-call sequence shift** — Jaccard distance between tool-use patterns pre/post boundary spikes while completion metrics stay green
3. **Semantic drift** — topic keyword overlap across sessions declines, indicating narrowed working knowledge

### Root Cause
Context compression after session rotation reduces the agent's effective knowledge base. The agent starts fresh, looks healthy, but responds differently to the same inputs. In a CrewAI crew, agents share outputs — if Agent A silently drifts, Agent B receives degraded inputs, and the crew-level output degrades in ways not attributable to any individual task failure.

### Evidence
- Reference implementation: [compression-monitor](https://github.com/agent-morrow/compression-monitor) — framework-agnostic toolkit measuring the three drift signals
- Lead-lag ordering of which agent drifts first identifies root cause

## Connection to ADE Theory

### Which ADE Principle Does This Violate?
**Cognitive Framework Lag (L4-B)**: The agent's cognitive framework (effective knowledge) lags behind its pre-compression state. The lag is invisible — task completion metrics remain green while behavioral quality degrades.

**Intelligence Entropy (L3)**: This is a direct manifestation — structured knowledge decays over time through context compression, increasing disorder invisibly.

### How Could ADE Prevent This?
1. **Behavioral Fingerprint Monitoring**: Compare pre/post session outputs for consistency
2. **Cross-Session Knowledge Preservation**: Persistent memory that survives compression
3. **Crew-Level Drift Detection**: Aggregate drift signals across all agents in a crew

## Fix / Defense
The compression-monitor toolkit provides continuous measurement of drift signals. A production fix requires:
- Pre/post session state comparison hooks
- Behavioral consistency thresholds that trigger alerts
- Persistent memory layers that survive compression

## Related Cases
- L1A-001 (Hermes cron memory fracture) — same root cause: cross-session knowledge loss
- L3A-001 (Hermes compression data loss) — same mechanism: compression without flush

## References
- GitHub Issue: [#5155](https://github.com/crewAIInc/crewAI/issues/5155)
- ADE Papers: [Silent Failure](https://arxiv.org/abs/2606.08162), [Intelligence Entropy](https://arxiv.org/abs/2606.18065)

---

## Metadata
- **Discovered by**: Community report (agent-morrow)
- **Reviewed by**: Dexing Liu
- **Last updated**: 2026-06-17
