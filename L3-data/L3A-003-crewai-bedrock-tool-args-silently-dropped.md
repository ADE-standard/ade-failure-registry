# L3A-003: CrewAI Bedrock Tool Arguments Silently Dropped

## Case ID
L3A-003

## Date
2026-04-12

## Source
- **Repository**: [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI)
- **Issue**: [#5275 - Bedrock tool call arguments silently dropped](https://github.com/crewAIInc/crewAI/issues/5275)
- **Architecture**: crewai

## Classification
- **Layer**: L3-A (Data Layer — Data Loss)
- **Severity**: High
- **Impact**: Tools invoked with empty input, TypeError or Pydantic ValidationError

## Description

### What Happened
When using CrewAI with AWS Bedrock LLM (Amazon Nova Pro), tool arguments are silently discarded. The LLM correctly calls the tool with the right arguments, but the tool receives an empty dict `{}`.

The data (tool arguments) is lost in transit between the LLM and the tool — a classic data loss in the argument passing channel.

### Root Cause
`_parse_native_tool_call` drops Bedrock Converse API tool arguments — always passes empty dict. The parsing layer between LLM output and tool invocation silently discards the argument data.

### Evidence
- Confirmed in CrewAI versions 1.9.3 and 1.13.0
- Same pattern in [#4972](https://github.com/crewAIInc/crewAI/issues/4972) (Bedrock tool arguments dropped)
- Tools receive `{}` instead of the arguments the LLM generated

## Connection to ADE Theory

### Which ADE Principle Does This Violate?
**Data Loss (L3-A)**: Tool arguments exist in the LLM output but are lost before reaching the tool. The data is irrecoverably dropped in the parsing layer.

**Channel Fracture (L1-A)**: The argument-passing channel between LLM and tool is fractured — arguments exist at the sender (LLM) but never arrive at the receiver (tool).

### How Could ADE Prevent This?
1. **Argument Integrity Check**: Compare LLM-generated arguments with tool-received arguments
2. **Parser Verification**: Validate that parsers preserve all fields
3. **Schema Enforcement**: Pydantic models should catch empty dicts for required fields

## Fix / Defense
- Fix `_parse_native_tool_call` to preserve Bedrock arguments
- Add argument count verification after parsing

## Related Cases
- L3A-001 (Hermes compression data loss) — same pattern: data exists then silently disappears
- L1A-001 (Hermes cron memory fracture) — same pattern: data in transit is lost

## References
- GitHub Issue: [#5275](https://github.com/crewAIInc/crewAI/issues/5275)
- Related: [#4972](https://github.com/crewAIInc/crewAI/issues/4972)
- ADE Paper: [Intelligence Entropy](https://arxiv.org/abs/2606.18065)

---

## Metadata
- **Discovered by**: Community report
- **Reviewed by**: Dexing Liu
- **Last updated**: 2026-06-17
