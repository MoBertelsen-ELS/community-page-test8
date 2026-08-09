# Design: community-status-transfer-errors

## Context
Community admins need confidence that harvested data from member instances is flowing into the community layer correctly. Today, failures and inconsistencies are hard to see, explain, and route back to the affected institution, which creates trust issues and developer/support dependency.

Start with a focused operational surface for unresolved transfer errors. It should show whether the harvester is running, which unresolved issues need attention, which member instance is affected, what the plain-language reason is, and what action the community admin can take.

A community admin can open Community status, see a clear harvester/status summary, triage unresolved transfer errors by affected instance, inspect the records affected by an error type, notify the relevant instance, and see that the notification task has been sent.

The current prototype explores a Pure Administrator page called `Community status`.

## Proposed Approach
- Validate the real product entry points, APIs, task routing, and permissions before implementation begins.
- Keep the first slice focused on unresolved transfer errors only.
- Reuse Graphene/Pure components where available and document any prototype-only departures.

## Product And Technical Decisions
- The page is available in the Administrator area under `Community`.
- The default view focuses on unresolved work, not time-scoped reporting.
- Errors are grouped by affected instance by default.
- No error is selected by default.
- Selecting an error type opens a detail pane with plain-language description, how-to-fix guidance, affected records, and suggested action.
- The manager can notify the affected instance.

## Dependencies And Validation
- Locate real Pure repository entry points for `Administrator > Community` navigation.
- Locate real harvester status/error APIs or data model.
- Locate existing task creation/task panel model.
- Confirm whether Graphene/Pure components cover the needed master-detail and task preview patterns.
- Confirm whether this is one story or needs an epic with separate stories for data API, admin UI, task routing, and permissions.

## Non-Goals
- full merge/deduplication management
- eventlog replacement
- single UUID reharvest action
- backend/confidential content policy changes
- cross-client priority/fingerprint conflict resolution
- customer-visible portal parity reporting

## Open Questions
- What is the exact boundary between transfer errors, merge/deduplication issues, and metadata parity issues?
- Which error types can be fixed by the member instance, and which require community-level or developer intervention?
- Should fingerprint and classified ID conflicts be represented as transfer errors, merge issues, or a separate parity/priority issue type?
- How should the UI explain cases where community data does not reflect a client-level change?
- What record-level event history is required to answer: did it harvest, did it fail, where did it fail, and why did it merge?
- Should notification create a task in the affected instance only, or also create a community-side follow-up state?
- What permissions should control who can see unresolved errors and notify instances?
- What is the product stance on backend/confidential content being synced to the community?
- Is single UUID reharvest a later action from this page, or a separate support/developer tool?
- Which data should be exportable from the record list?
