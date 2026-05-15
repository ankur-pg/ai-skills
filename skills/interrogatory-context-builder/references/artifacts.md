# Context Artifact Templates

## Context Brief

Use this after Build Mode reaches enough clarity for planning or implementation.
Omit sections that are not relevant, but keep material gaps visible.

```markdown
# Context Brief: <task name>

## Objective

- <confirmed user or business outcome>

## Non-Goals

- <explicitly excluded scope>

## Current State

- <existing system, workflow, document, or process context>

## Decisions

| Decision | Rationale | Rejected Alternatives | Owner |
| --- | --- | --- | --- |
| <decision> | <why> | <what was rejected and why> | <owner> |

## Constraints

- <technical, product, data, security, compliance, or operational constraint>

## Business Rules and Edge Cases

- <rule or edge case>

## Interfaces and Data

- <system, contract, event, table, API, owner, or data source>

## Risks

- <risk and impact>

## Open Questions

- [ ] <question, owner if known>

## Validation Expectations

- <tests, checks, metrics, approval gates, rollout controls, or review needs>

## Next Actions

- <smallest useful next step>

## Handoff Note

The most important unresolved constraint or decision is <constraint or decision> because <reason>.
```

## Spec Review Report

Use this when validating an existing document.

```markdown
# Context Review: <document or task name>

## Confirmed

- <claim confirmed by the expert or authoritative source>

## Conflicts

- <document claim> conflicts with <source or expert answer>.

## Missing Decisions

- <decision still needed before implementation>

## Ambiguous Terms

- <term> needs a precise definition.

## Unverified Assumptions

- <assumption that should not be treated as a requirement yet>

## Open Risks

- <risk and suggested validation>

## Recommended Next Interview Question

- <one focused question>

## Handoff Note

The most important unresolved constraint or decision is <constraint or decision> because <reason>.
```

## Session Handoff

Use this when the user wants to pause and resume later.

```markdown
# Session Handoff: <task name>

## Goal

- <what the next session should accomplish>

## Confirmed Context

- <fact>

## Decisions and Rationale

| Decision | Rationale | Owner |
| --- | --- | --- |
| <decision> | <why> | <owner> |

## Current State

- <what has been done so far>

## Open Questions

- [ ] <question>

## Next Action

- <one next step>
```
