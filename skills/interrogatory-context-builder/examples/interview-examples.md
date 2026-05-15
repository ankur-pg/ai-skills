# Interrogatory Context Builder Examples

## Build Mode

- User: "I want to build saved-search alerts.
  Interview me before we design it."
- Assistant: "Build Mode.
  First question: What user outcome must saved-search alerts support in the first release?"

## Review Mode

- User: "Review this spec with me.
  It says alerts are near real time, duplicate emails are acceptable, and users can unsubscribe later."
- Assistant: "Review Mode.
  I see three high-risk claims: near-real-time delivery, acceptable duplicate emails, and deferred unsubscribe behavior.
  First question: Which stakeholder has approved duplicate emails as acceptable?"

## Handoff Mode

- User: "We decided daily email only, no push notifications.
  Search owns matching.
  Marketing owns templates.
  Singapore is the first rollout.
  Create a handoff."
- Assistant: "## Session Handoff: Saved-Search Alerts

  ## Goal

  - Prepare the next session to design daily email alerts for saved searches.

  ## Confirmed Context

  - Users can save a property search.
  - The first release sends daily email alerts for new matches.
  - Push notifications are out of scope.
  - Singapore is the first rollout market.

  ## Decisions and Rationale

  | Decision | Rationale | Owner |
  | --- | --- | --- |
  | Daily email alerts only | Keeps the first release narrow and avoids push-notification complexity | Product owner |
  | Search owns matching | Matching rules already belong to the search domain | Search API owner |
  | Marketing owns templates | Email copy and template approvals sit with marketing | Marketing owner |

  ## Open Questions

  - [ ] What exactly counts as a new matching listing?
  - [ ] What unsubscribe audit or compliance requirements apply?
  - [ ] Which delivery metrics are required for launch readiness?

  ## Next Action

  - Confirm matching semantics with the Search API owner.

  ## Handoff Note

  The most important unresolved constraint is the definition of a new matching listing because it affects correctness, notification volume, and user trust."
