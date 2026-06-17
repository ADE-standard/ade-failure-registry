# Case Study Template

Copy this template and fill in all sections.

```markdown
# {ID}: {Title}

## Case ID
{L{X}{Y}-{NNN}} — See [TAXONOMY.md](../TAXONOMY.md) for classification codes.

## Date
{YYYY-MM-DD}

## Source
- **Repository**: [{name}]({url})
- **Issue**: [#{number}]({issue_url})
- **Architecture**: {hermes|langchain|crewai|autogpt|openai|claude|custom}

## Classification
- **Layer**: L{1-5}-{A/B/C} — {layer name}
- **Severity**: {Critical|High|Medium|Low}
- **Impact**: {one-sentence summary}

## Description

### What Happened
{Factual description of the incident, 2-4 paragraphs}

### Root Cause
{Technical root cause analysis — go to code/config level}

### Evidence
{Logs, screenshots, reproduction steps, commit hashes}

## Connection to ADE Theory

### Which ADE Principle Does This Violate?
{Name the principle(s) and explain the connection}

### How Could ADE Prevent This?
{Specific ADE mechanisms that would have caught this}

## Fix / Defense
{What was done or should be done to prevent recurrence}

## Related Cases
{Links to related cases in this registry}

## References
- GitHub Issue: [#{number}]({url})
- ADE Papers: {relevant paper links}

---

## Metadata
- **Discovered by**: {name / "Community report" / "Production incident"}
- **Reviewed by**: {name}
- **Last updated**: {YYYY-MM-DD}
```
