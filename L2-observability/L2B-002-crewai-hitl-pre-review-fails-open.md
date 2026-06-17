# L2B-002: CrewAI HITL Pre-review Fails Open — Silent Safeguard Bypass

## Case ID
L2B-002

## Date
2026-05-11

## Source
- **Repository**: [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI)
- **Issue**: [#5725 - HITL pre-review fails open and silently bypasses automated safeguards](https://github.com/crewAIInc/crewAI/issues/5725)
- **Architecture**: crewai

## Classification
- **Layer**: L2-B (Observability Layer — Success Illusion)
- **Severity**: Critical
- **Impact**: Safety mechanism reports "reviewed" when review never happened

## Description

### What Happened
When `@human_feedback(..., learn=True)` is used and lessons are recalled, the pre-review step applies lessons via LLM before presenting output for human feedback. If any exception occurs (network, auth, LLM error), the implementation catches all exceptions and returns the original output — silently bypassing the safeguard.

Callers cannot distinguish between:
- Pre-reviewed output (safeguard applied)
- Raw model output (safeguard bypassed)

### Root Cause
```python
try:
    reviewed = apply_lessons(method_output, recalled_lessons)
    return reviewed
except Exception:
    return method_output  # ← Silent fallback: no log, no signal
```

The `except` block returns success without any indication that the pre-review step was skipped.

### Evidence
- Reproduction: force LLM call to raise RuntimeError → pre-review silently skipped
- No exception propagated, no warning logged, no structured signal emitted

## Connection to ADE Theory

### Which ADE Principle Does This Violate?
**Success Illusion (L2-B)**: The system reports "output is ready for human review" but the pre-review step never executed. The success signal is an illusion.

**BCP (Behavior Contract Protocol)**: The behavior contract says "pre-review then human review" — the system delivers "skip pre-review, go to human review" while reporting success.

### How Could ADE Prevent This?
1. **Fail-Closed Default**: If pre-review fails, block human review — don't skip it
2. **Metadata Propagation**: Return `pre_review_applied=False` so callers know
3. **Structured Error Signals**: Emit events that downstream components can observe

## Fix / Defense
- Propagate exception (fail closed)
- Or: log at WARNING with exc_info=True
- Or: return metadata `pre_review_applied=False`

## Related Cases
- L5A-001 (Hermes email auto-reply) — same pattern: safety mechanism silently bypassed
- L2C-001 (Hermes cron approval deadlock) — safety mechanism creates worse failure

## References
- GitHub Issue: [#5725](https://github.com/crewAIInc/crewAI/issues/5725)
- ADE Paper: [Silent Failure](https://arxiv.org/abs/2606.08162)

---

## Metadata
- **Discovered by**: Community report
- **Reviewed by**: Dexing Liu
- **Last updated**: 2026-06-17
