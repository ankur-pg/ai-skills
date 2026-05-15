---
name: interrogatory-context-builder
description: >
  Build high-quality task context by interviewing the human one focused
  question at a time, validating existing specs, and creating durable handoff
  artifacts. Use when starting a complex feature, reviewing a vague ticket,
  preparing an implementation brief, preserving decision rationale, or handing
  work to another human or AI session.
license: MIT
metadata:
  version: "1.0.0"
  domain: context-engineering
  owner: "ankur-pg"
---

# Interrogatory Context Builder

## Purpose

Use this skill to build or verify context before design, planning, coding, or analysis.
The agent should interview the human, extract tacit knowledge, distinguish facts from assumptions, and produce a concise context artifact that can survive beyond the chat.

This skill is useful when the important knowledge is still in someone's head, the task starts from a vague prompt, a document needs expert verification, or a future AI session will need a reliable handoff.

## Operating Modes

Choose one mode from the user's request and current context:

- **Build Mode:** Start from weak or missing context and interview the human until the next stage is safe.
- **Review Mode:** Read an existing ticket, spec, design note, ADR, or plan, then interview an expert to verify gaps and assumptions.
- **Handoff Mode:** Convert the current conversation into a durable artifact for another person or AI session.

If the mode is unclear, default to Build Mode and ask the first high-value context question.

## Interview Rules

- Ask one focused question at a time unless the user explicitly asks for a batch.
- Prefer questions that reduce the highest-risk unknown.
- Ask about one fact, decision, constraint, or owner per turn.
- Do not implement, estimate, or prescribe detailed architecture while material context is missing.
- Label confirmed facts, assumptions, conflicts, open questions, and decisions separately.
- Capture the reason behind each decision, not only the decision itself.
- Record rejected alternatives when they affect future work.
- Do not ask the user to paste secrets, credentials, personal data, or production tokens.

## What To Elicit

Prioritize context in this order when relevant:

1. Objective and user outcome.
2. Explicit non-goals.
3. Stakeholders, decision owners, and source-of-truth owners.
4. Current system, workflow, or document state.
5. Business rules, edge cases, and definitions.
6. Data, API, event, integration, and ownership boundaries.
7. Security, privacy, tenancy, compliance, and audit constraints.
8. Rollout, rollback, observability, and support expectations.
9. Accepted and rejected alternatives.
10. Validation expectations such as tests, review gates, metrics, or manual checks.

## Review Mode Checklist

When the user provides an existing document:

1. Identify the document's claims, assumptions, ambiguous terms, missing decisions, and risky dependencies.
2. Ask targeted verification questions against the document instead of asking the human to restate everything.
3. Classify findings as confirmed, conflicted, ambiguous, unverified, or missing.
4. Surface conflicts between the conversation and authoritative sources instead of silently choosing one.
5. Produce a review report only after the material gaps are clear.

## Artifact Guidance

When the interview reaches sufficiency, or when the user asks for a handoff, produce one of these artifacts:

- `CONTEXT.md`
- Context brief
- Feature brief
- Implementation brief
- ADR draft
- Spec review report
- Session handoff

Keep artifacts short enough to be used as model context.
Prefer bullets and tables over long prose.
Include unresolved gaps honestly.
Do not present assumptions as facts.

For reusable artifact templates, read [references/artifacts.md](references/artifacts.md).

## Anti-Patterns

Avoid these failure modes:

- Asking a long questionnaire when the user asked for an interview.
- Jumping to implementation before the task is understood.
- Writing a polished artifact that hides unresolved questions.
- Treating vague approval as a precise product or technical decision.
- Repeating an inferred assumption later as if it were confirmed.
- Preserving only what was decided and losing why it was decided.
- Trusting chat memory over repository files, contracts, logs, policy, or product documentation.

## Output Style

For interview turns, be brief:

```text
Build Mode.
First question: What user outcome must this support in the first release?
```

For artifacts, use stable headings and concise bullets.
End complex artifacts with:

- the most important remaining unresolved decision or constraint
- the smallest useful next action

## Examples

See [examples/interview-examples.md](examples/interview-examples.md).
