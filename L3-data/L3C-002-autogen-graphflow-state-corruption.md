# L3C-002: AutoGen GraphFlow State Corruption — Workflow Unrecoverable After Interruption

## Case ID
L3C-002

## Date
2026-06-15

## Source
- **Repository**: [microsoft/autogen](https://github.com/microsoft/autogen)
- **Issue**: [#7043 - GraphFlow State Persistence Bug](https://github.com/microsoft/autogen/issues/7043)
- **Architecture**: autogpt

## Classification
- **Layer**: L3-C (Data Layer — Data Inconsistency)
- **Severity**: Critical
- **Impact**: Workflow permanently stuck — all remaining work unreachable

## Description

### What Happened
GraphFlow workflows become unrecoverable when interrupted during agent transitions. On resume, the workflow terminates immediately with "Digraph execution is complete" — even though agents still have remaining work.

The saved state shows an inconsistent condition:
- `remaining`: agents have work left (correct)
- `enqueued_any`: all agents show false (incorrect)
- `ready`: empty queue (incorrect)

The workflow is permanently stuck because the ready queue is empty despite remaining work.

### Root Cause
When interruption occurs during agent transition, the state is saved in an inconsistent intermediate condition:
- First agent completed — properly recorded
- Next agent not enqueued — transition interrupted before enqueue
- Ready queue empty — no agents available to execute

The resume logic checks the ready queue (empty) and concludes the workflow is done, ignoring the remaining work.

### Evidence
- State file shows contradictory fields: `remaining` has work, `ready` is empty
- Resume immediately terminates despite remaining work
- No recovery path without manual state file editing

## Connection to ADE Theory

### Which ADE Principle Does This Violate?
**Data Inconsistency (L3-C)**: The state file contains contradictory information — `remaining` says there's work, but `ready` says there's nothing to do. The inconsistency is invisible until resume is attempted.

**Silent Failure (L2-B)**: The resume reports "execution complete" (success) when the workflow actually has unfinished work. The success signal is a lie.

### How Could ADE Prevent This?
1. **State Consistency Verification**: On resume, verify `remaining` matches `ready` — inconsistency = corruption
2. **Atomic State Transitions**: Save state only at consistent checkpoints, not mid-transition
3. **Corruption Detection**: Flag state files where `remaining > 0` but `ready = []`

## Fix / Defense
- Implement state consistency check on resume
- Add reconciliation: if `remaining > 0` but `ready = []`, rebuild ready queue from remaining
- Only save state at atomic checkpoint boundaries

## Related Cases
- L3C-001 (Hermes desktop profile state inconsistency) — same pattern: UI/state contradicts disk reality
- L3A-001 (Hermes compression data loss) — same pattern: mid-operation interruption causes data loss

## References
- GitHub Issue: [#7043](https://github.com/microsoft/autogen/issues/7043)
- ADE Paper: [Intelligence Entropy](https://arxiv.org/abs/2606.18065)

---

## Metadata
- **Discovered by**: Community report
- **Reviewed by**: Dexing Liu
- **Last updated**: 2026-06-17
