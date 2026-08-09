# Proposal: community-status-transfer-errors

## Why
Community admins need confidence that harvested data from member instances is flowing into the community layer correctly. Today, failures and inconsistencies are hard to see, explain, and route back to the affected institution, which creates trust issues and developer/support dependency.

## What Changes
Build a `Community status` administrator surface for unresolved Community Harvester transfer errors.

## Capabilities
### New Capabilities
- `community-status-transfer-errors`: Community Status

### User-Facing Behavior
- The page is available in the Administrator area under `Community`.
- The page communicates whether the harvester is running or interrupted.
- The page shows latest harvest and next harvest timestamps as supporting context.
- The default view focuses on unresolved work, not time-scoped reporting.
- Errors are grouped by affected instance by default.
- A manager can filter by time range and content type.
- No error is selected by default.
- Selecting an instance reveals its unresolved error types.
- Selecting an error type opens a detail pane with plain-language description, how-to-fix guidance, affected records, and suggested action.
- The manager can notify the affected instance.
- The notify flow previews the task item the instance will receive.
- After sending, the UI indicates that the task has been sent.
- Header and content alignment must work responsively.
- Design should use Graphene/Pure components where available; prototype-only visual departures must be called out before product implementation.

## Out of Scope
- full merge/deduplication management
- eventlog replacement
- single UUID reharvest action
- backend/confidential content policy changes
- cross-client priority/fingerprint conflict resolution
- customer-visible portal parity reporting

## Dependencies Before Implementation
- Locate real Pure repository entry points for `Administrator > Community` navigation.
- Locate real harvester status/error APIs or data model.
- Locate existing task creation/task panel model.
- Confirm whether Graphene/Pure components cover the needed master-detail and task preview patterns.
- Confirm whether this is one story or needs an epic with separate stories for data API, admin UI, task routing, and permissions.

## Review Notes
- This is an initial enterprise draft derived from the recorded enterprise source brief.
- Review the product fit and production entry points in the intent PR.
- What is the exact boundary between transfer errors, merge/deduplication issues, and metadata parity issues?
- Which error types can be fixed by the member instance, and which require community-level or developer intervention?
- Should fingerprint and classified ID conflicts be represented as transfer errors, merge issues, or a separate parity/priority issue type?
- How should the UI explain cases where community data does not reflect a client-level change?
- What record-level event history is required to answer: did it harvest, did it fail, where did it fail, and why did it merge?
