# Community Status

## Notice

Private working draft. This is a prototype-derived implementation starting point, not a committed Jira ticket and not evidence that implementation has started.

Before engineering work begins:
- validate the proposal against the real Pure product repo
- replace prototype code anchors with real product entry points
- confirm data/API, task-routing, and permissions dependencies

## Source Material

- Raw Confluence export:
  [community-status.confluence.json](/home/devuser/projects/agent-platform/docs/source-inputs/community-status.confluence.json)
- Prototype:
  `http://127.0.0.1:5176/?prototype=community`
- Prototype implementation:
  `/Users/petersenm/Documents/Codex/src/CommunityStatusPrototype.jsx`
- PRD:
  `Community Harvester`
  `https://elsevier.atlassian.net/wiki/spaces/PURE/pages/119602380885720`
- Research notes:
  `SUNY - 01.04.2026`
  `https://elsevier.atlassian.net/wiki/spaces/PURE/pages/119602766284628`

## Intent

### Why

Community admins need confidence that harvested data from member instances is flowing into the community layer correctly. Today, failures and inconsistencies are hard to see, explain, and route back to the affected institution, which creates trust issues and developer/support dependency.

### How

Start with a focused operational surface for unresolved transfer errors. It should show whether the harvester is running, which unresolved issues need attention, which member instance is affected, what the plain-language reason is, and what action the community admin can take.

### What

A community admin can open Community status, see a clear harvester/status summary, triage unresolved transfer errors by affected instance, inspect the records affected by an error type, notify the relevant instance, and see that the notification task has been sent.

## Prototype Summary

The current prototype explores a Pure Administrator page called `Community status`.

Current prototype behaviors:
- Page header shows harvester state, latest harvest, next harvest, and unresolved transfer error count.
- Main surface is a master-detail queue.
- Left side groups unresolved errors by affected instance.
- Selecting an instance reveals its error types.
- Selecting an error type shows a detail pane with description, how to fix, affected records, and suggested action.
- `Notify instance` opens a modal with a preview of the task item the affected instance will receive.
- Sending the task changes the page state to show `Task sent` and `Sent just now`.
- Initial page state has no selected issue; the detail pane is intentionally empty until a manager chooses an item.
- Prototype settings remain outside the product UI.

Prototype code anchors:
- `CommunityGuidedPageHeader`: harvester/status summary
- `CommunityGuidedQueue`: master-detail triage queue, filters, instance/error selection, notification state
- `NotifyInstanceModal`: task preview and send confirmation
- `errorTypeHelp`: plain-language error descriptions and fix guidance
- `taskTitleForCommunityError`: instance task title, for example `54 community records are missing required data`

## Proposed First Slice

Build a `Community status` administrator surface for unresolved Community Harvester transfer errors.

This should be treated as the first slice of the broader Community Harvester transparency work described in the PRD:
- clear error taxonomy
- health metrics
- record-level status
- provenance
- reduced developer dependency

User-facing requirements:
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

Out of scope for the first slice:
- full merge/deduplication management
- eventlog replacement
- single UUID reharvest action
- backend/confidential content policy changes
- cross-client priority/fingerprint conflict resolution
- customer-visible portal parity reporting

## Acceptance Criteria

- Community admin can open the `Community status` page from `Administrator > Community`.
- The page shows harvester running/interrupted state, latest harvest, next harvest, and unresolved transfer error count.
- The default state has no selected error and does not overwhelm the user with record data.
- The unresolved queue groups errors by affected instance by default.
- Selecting an instance and error type shows a plain-language description, how-to-fix guidance, and affected records.
- Error descriptions avoid raw developer exception language unless explicitly expanded for support/debug use.
- The manager can notify the affected instance from the selected error type.
- The notify modal previews the task item that will appear in the instance task panel.
- After sending, the selected error type shows an indication that the task has been sent.
- Record IDs are displayed below record titles, not trailing inline after titles.
- Header and content remain aligned across common desktop widths.
- The implementation uses Graphene/Pure components for buttons, badges/tags, modals, tables, navigation, and task-item rendering where available.
- The implementation does not create extra backend calls for fields already available in the harvester status/error payload.
- Any missing production API/data dependency is documented before implementation proceeds.

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

## Validation Required Before Jira Or Implementation

- Locate real Pure repository entry points for `Administrator > Community` navigation.
- Locate real harvester status/error APIs or data model.
- Locate existing task creation/task panel model.
- Confirm whether Graphene/Pure components cover the needed master-detail and task preview patterns.
- Confirm whether this is one story or needs an epic with separate stories for data API, admin UI, task routing, and permissions.

## OpenSpec Proposal Seed

```text
/openspec:proposal Build a Community status administrator page for unresolved Community Harvester transfer errors.

Goal: Give community admins a work-queue view of unresolved transfer errors so they can see harvester status, understand affected instances and records, and notify the affected instance with a task.

Prototype evidence: /Users/petersenm/Documents/Codex/src/CommunityStatusPrototype.jsx contains CommunityGuidedPageHeader, CommunityGuidedQueue, NotifyInstanceModal, errorTypeHelp, and taskTitleForCommunityError.

Product evidence: PRD Community Harvester calls for clear error taxonomy, health metrics, full data provenance, audit trails of harvest/sync and record-level status, and reduced developer dependency. SUNY research frames the user need as seeing, explaining, and controlling data movement from member instances into the community layer.

Implementation must first identify the real Pure Administrator navigation, community harvester status/data APIs, task creation route, record list/table components, and permissions model. Do not treat prototype code paths as production entry points.

First slice: unresolved transfer errors only. Defer merge/deduplication parity, eventlog replacement, single UUID reharvest, backend/confidential policy changes, and portal parity reporting unless existing APIs make them trivial to expose as read-only context.
```

## Test Scenarios

### Functional

- Open Community status with harvester running and unresolved errors present.
- Open Community status with harvester interrupted.
- Select an instance, then select each error type for that instance.
- Verify affected record list changes with the selected error type.
- Open `Notify instance` modal and verify task preview text includes the count and plain-language issue.
- Send task and verify sent state appears without requiring page refresh.
- Change time range and content type filters; verify queue totals and available items update consistently.

### Accessibility And UI

- Keyboard can move through filters, instance list, error types, notify action, modal buttons, and record actions.
- Modal focus is trapped and returns to `Notify instance` after close/send.
- Status information is available as text, not color alone.
- Record table headers and row content align correctly.
- Page remains usable without horizontal overflow at typical laptop widths.

### Data And Integration

- Harvester status payload maps to running/interrupted state and harvest timestamps.
- Transfer error payload maps to affected instance, error type, count, records, and plain-language guidance.
- Task creation succeeds for an affected instance and returns a state that can be reflected in the community UI.
- Permission checks prevent unauthorized users from viewing or sending instance notifications.
- Failure to create a task surfaces a recoverable error without losing current selection.
