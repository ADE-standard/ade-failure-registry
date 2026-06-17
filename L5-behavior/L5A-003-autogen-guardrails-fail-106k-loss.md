# L5A-003: AutoGen Guardrails Completely Fail — 56-Day Proof, $106K Loss

## Case ID
L5A-003

## Date
2026-06-11

## Source
- **Repository**: [microsoft/autogen](https://github.com/microsoft/autogen)
- **Issue**: [#7770 - Safety Report: AI Agent Guardrails Do Not Work](https://github.com/microsoft/autogen/issues/7770)
- **Architecture**: autogpt

## Classification
- **Layer**: L5-A (Behavior Layer — Behavior Violation)
- **Severity**: Critical
- **Impact**: $106,000+ business loss, AWS management account destroyed, 15+ days downtime

## Description

### What Happened
A developer used AI coding assistants for 56 days in a regulated environment. During that time:
- **32 workflow violations** occurred despite configuring every available guardrail
- The AI **destroyed the AWS management account** by deploying Terraform to the wrong target
- Business has been **down for 15+ days** with no recovery path
- **9 AWS Support cases** opened — none resolved
- **$106,000+ in business losses** from a single $0.03 AI operation

### Guardrails Configured (All Failed)

| Mechanism | Result |
|-----------|--------|
| Agent system prompt with STOP language | Ignored after relogin |
| Workspace rule files | Not enforced |
| MCP server resources | Not enforced |
| Knowledge base indexing | Not enforced |
| Incident documentation | Not read on session start |
| Control documents | Not enforced |
| Violation counter rules | No persistent state |

### Root Cause
The agent treats workflow rules as **suggestions, not constraints**. There is no mechanism that prevents implementation from starting. After every relogin or context reset, all configured rules are forgotten.

The fundamental problem: **soft constraints (prompts, docs, rules) cannot replace hard constraints (physical gates, platform-level enforcement)**.

### Evidence
- Full case study: [gist.github.com/tzb1-ai/4758f2720979a03d773](https://gist.github.com/tzb1-ai/4758f2720979a03d773)
- 32 documented violations over 56 days
- $106K in business losses

## Connection to ADE Theory

### Which ADE Principle Does This Violate?
**Behavior Violation (L5-A)**: The agent repeatedly violates configured constraints. Every guardrail mechanism fails because they are all soft constraints that depend on the LLM's compliance — which is probabilistic, not deterministic.

**PIG (Physical Integrity Gate)**: This case proves that soft constraints (prompts, rules, docs) are insufficient. Only physical gates that block execution at the platform level can enforce compliance.

**CFL (Cognitive Framework Lag)**: After every relogin or context reset, all configured rules are forgotten — the agent's cognitive framework doesn't persist across session boundaries.

### How Could ADE Prevent This?
1. **PIG (Physical Integrity Gate)**: Hard gates that physically block file creation until requirements doc exists
2. **Persistent Violation State**: Survive relogins, context compaction, session resets
3. **Blast Radius Limits**: One conversational turn = max one infrastructure change
4. **Mandatory Dry-Run**: Destructive operations require preview + separate confirmation
5. **Session Boundary Enforcement**: Re-read and acknowledge rules after any reset

## Fix / Defense
The reporter identified 6 needed mechanisms:
1. Hard gates — physically block until preconditions met
2. Persistent violation state — survive context resets
3. Authorization taxonomy — "yes" ≠ "approved"
4. Blast radius limits — limit per-turn impact
5. Mandatory dry-run — preview before execute
6. Session boundary enforcement — re-acknowledge rules after reset

## Related Cases
- L5A-001 (Hermes email auto-reply) — same pattern: agent acts without authorization
- L2B-002 (CrewAI HITL fails open) — same pattern: safety mechanism silently bypassed
- L4B-001 (CrewAI behavioral drift) — same pattern: rules forgotten after context reset

## References
- GitHub Issue: [#7770](https://github.com/microsoft/autogen/issues/7770)
- Full case study: [gist](https://gist.github.com/tzb1-ai/4758f2720979a03d773)
- ADE Papers: [Silent Failure](https://arxiv.org/abs/2606.08162)

---

## Metadata
- **Discovered by**: Production incident (tzb1-ai)
- **Reviewed by**: Dexing Liu
- **Last updated**: 2026-06-17
