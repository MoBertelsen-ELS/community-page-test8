# Tasks: community-status-transfer-errors

## 1. Validate Production Dependencies

- [ ] 1.1 Locate real Pure repository entry points for `Administrator > Community` navigation.
- [ ] 1.2 Locate real harvester status/error APIs or data model.
- [ ] 1.3 Locate existing task creation/task panel model.
- [ ] 1.4 Confirm whether Graphene/Pure components cover the needed master-detail and task preview patterns.
- [ ] 1.5 Confirm whether this is one story or needs an epic with separate stories for data API, admin UI, task routing, and permissions.

## 2. Author The Intent Package

- [ ] 2.1 Author `proposal.md` with concrete product-facing content.
- [ ] 2.2 Author `design.md` with concrete product-facing content.
- [ ] 2.3 Author `tasks.md` with concrete product-facing content.

## 3. Define The Delta Spec

- [ ] 3.1 Capture this behavior precisely in the delta spec: Community admin can open the Community status page from Administrator > Community.
- [ ] 3.2 Capture this behavior precisely in the delta spec: The page shows harvester running/interrupted state, latest harvest, next harvest, and unresolved transfer error count.
- [ ] 3.3 Capture this behavior precisely in the delta spec: The default state has no selected error and does not overwhelm the user with record data.
- [ ] 3.4 Capture this behavior precisely in the delta spec: The unresolved queue groups errors by affected instance by default.
- [ ] 3.5 Capture this behavior precisely in the delta spec: A manager can filter unresolved errors by time range and content type, and the queue updates consistently with the applied filters.
- [ ] 3.6 Capture this behavior precisely in the delta spec: Selecting an instance and error type shows a plain-language description, how-to-fix guidance, and affected records.
- [ ] 3.7 Capture this behavior precisely in the delta spec: The error-type detail pane includes a suggested action alongside the plain-language description, how-to-fix guidance, and affected records.
- [ ] 3.8 Capture this behavior precisely in the delta spec: Error descriptions avoid raw developer exception language unless explicitly expanded for support/debug use.
- [ ] 3.9 Capture this behavior precisely in the delta spec: The manager can notify the affected instance from the selected error type.
- [ ] 3.10 Capture this behavior precisely in the delta spec: The notify modal previews the task item that will appear in the instance task panel.
- [ ] 3.11 Capture this behavior precisely in the delta spec: After sending, the selected error type shows an indication that the task has been sent.
- [ ] 3.12 Capture this behavior precisely in the delta spec: Record IDs are displayed below record titles, not trailing inline after titles.
- [ ] 3.13 Capture this behavior precisely in the delta spec: Header and content remain aligned across common desktop widths.
- [ ] 3.14 Capture this behavior precisely in the delta spec: The implementation uses Graphene/Pure components for buttons, badges/tags, modals, tables, navigation, and task-item rendering where available.
- [ ] 3.15 Capture this behavior precisely in the delta spec: The implementation does not create extra backend calls for fields already available in the harvester status/error payload.
- [ ] 3.16 Capture this behavior precisely in the delta spec: Any missing production API/data dependency is documented before implementation proceeds.

## 4. Prepare Review Readiness

- [ ] 4.1 Ensure the intent package covers this review checkpoint: Community admin can open the `Community status` page from `Administrator > Community`.
- [ ] 4.2 Ensure the intent package covers this review checkpoint: The page shows harvester running/interrupted state, latest harvest, next harvest, and unresolved transfer error count.
- [ ] 4.3 Ensure the intent package covers this review checkpoint: The default state has no selected error and does not overwhelm the user with record data.
- [ ] 4.4 Ensure the intent package covers this review checkpoint: The unresolved queue groups errors by affected instance by default.
- [ ] 4.5 Ensure the intent package covers this review checkpoint: Selecting an instance and error type shows a plain-language description, how-to-fix guidance, and affected records.
- [ ] 4.6 Ensure the intent package covers this review checkpoint: Error descriptions avoid raw developer exception language unless explicitly expanded for support/debug use.
- [ ] 4.7 Ensure the intent package covers this review checkpoint: The manager can notify the affected instance from the selected error type.
- [ ] 4.8 Ensure the intent package covers this review checkpoint: The notify modal previews the task item that will appear in the instance task panel.
- [ ] 4.9 Ensure the intent package covers this review checkpoint: After sending, the selected error type shows an indication that the task has been sent.
- [ ] 4.10 Ensure the intent package covers this review checkpoint: Record IDs are displayed below record titles, not trailing inline after titles.
- [ ] 4.11 Ensure the intent package covers this review checkpoint: Header and content remain aligned across common desktop widths.
- [ ] 4.12 Ensure the intent package covers this review checkpoint: The implementation uses Graphene/Pure components for buttons, badges/tags, modals, tables, navigation, and task-item rendering where available.
- [ ] 4.13 Ensure the intent package covers this review checkpoint: The implementation does not create extra backend calls for fields already available in the harvester status/error payload.
- [ ] 4.14 Ensure the intent package covers this review checkpoint: Any missing production API/data dependency is documented before implementation proceeds.

## 5. Guard Rails

- [ ] 5.1 Add a PR-blocking lint/code-review gate that fails the diff if the community-status page module issues a new network/API call to fetch a field already declared in the payload contract this intent package documents: harvester running/interrupted state, latest harvest timestamp, next harvest timestamp, unresolved transfer error count, affected instance, error type, count, affected records, and plain-language guidance. This gate is committed now and does not wait on Task 1.2's dependency validation; when the real payload is confirmed, extend the same gate's field list rather than replacing it.

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

## Verification/Proof Plan

- [ ] Community admin can open the Community status page from Administrator > Community: Navigation test asserting the `Administrator > Community` menu includes a `Community status` entry that routes to the page for an authorized admin session.
- [ ] The page shows harvester running/interrupted state, latest harvest, next harvest, and unresolved transfer error count: Component test asserting the page renders `Running`/`Interrupted` badge text, latest harvest timestamp, next harvest timestamp, and unresolved error count from the harvester status payload state (covers Test Scenarios: harvester running / harvester interrupted).
- [ ] The default state has no selected error and does not overwhelm the user with record data: Component test rendering the page with unresolved errors present and asserting the detail pane and record list are empty with no error type marked selected on initial load.
- [ ] The unresolved queue groups errors by affected instance by default: Component test seeding errors across multiple instances and asserting the queue renders one group per affected instance with its errors nested underneath.
- [ ] A manager can filter unresolved errors by time range and content type: E2E test applying a time range and content type filter and asserting queue group totals and available instances/error types update to match the filtered data set (covers Test Scenarios: change time range and content type filters).
- [ ] Selecting an instance and error type shows a plain-language description, how-to-fix guidance, and affected records: E2E test selecting an instance, then an error type, and asserting the detail pane shows description, fix guidance, and the matching affected-records list (covers Test Scenarios: select an instance, then select each error type).
- [ ] The error-type detail pane includes a suggested action alongside the plain-language description, how-to-fix guidance, and affected records: Component test asserting the detail pane for a selected error type renders a suggested action element alongside description, fix guidance, and affected records.
- [ ] Error descriptions avoid raw developer exception language unless explicitly expanded for support/debug use: Content/component test asserting error-type detail text matches curated plain-language copy and that raw exception/stack trace text is shown only behind an explicit support/debug expansion control.
- [ ] The manager can notify the affected instance from the selected error type: E2E test that, with an error type selected, clicks `Notify instance` and asserts the notify flow opens scoped to that error type's affected instance.
- [ ] The notify modal previews the task item that will appear in the instance task panel: E2E test opening the `Notify instance` modal and asserting the preview text includes the affected record count and the plain-language issue (covers Test Scenarios: verify task preview text).
- [ ] After sending, the selected error type shows an indication that the task has been sent: E2E test sending the notification task and asserting the selected error type shows `Task sent` / `Sent just now` without a page reload (covers Test Scenarios: verify sent state appears without requiring page refresh).
- [ ] Record IDs are displayed below record titles, not trailing inline after titles: Component/visual test asserting each record row renders the record ID on a line below the title element, not concatenated inline after the title text.
- [ ] Header and content remain aligned across common desktop widths: Responsive/visual regression test rendering the page at common desktop viewport widths and asserting header and content regions remain aligned with no horizontal overflow (covers Test Scenarios: usable without horizontal overflow at typical laptop widths).
- [ ] The implementation uses Graphene/Pure components for buttons, badges/tags, modals, tables, navigation, and task-item rendering where available: Code review checklist plus component test asserting buttons, badges/tags, modals, tables, navigation, and task-item elements are built from Graphene/Pure component imports, with any prototype-only departure documented in `design.md`.
- [ ] The implementation does not create extra backend calls for fields already available in the harvester status/error payload: PR-blocking lint/code-review gate (Task 5.1) that rejects any diff adding a fetch/query in the community-status page module for `runningState`, `latestHarvestAt`, `nextHarvestAt`, `unresolvedErrorCount`, `affectedInstance`, `errorType`, `errorCount`, `affectedRecords`, or `plainLanguageGuidance` (or their real-endpoint equivalents once Task 1.2 confirms field names) — these are the fields already documented across `proposal.md`, `design.md`, the spec, and Test Scenarios as available on the harvester status/error payload. This gate is committed today and stands independent of Task 1.2; confirming the real payload only updates the field-name mapping the gate checks against, not whether the gate exists.
- [ ] Any missing production API/data dependency is documented before implementation proceeds: Process check — confirm the Dependencies Before Implementation / Open Questions sections in `proposal.md` and `design.md` are reviewed and updated to record any unlocated dependency, tracked via completion status of Tasks 1.1-1.5.

<!-- agent-platform:intent-proof-plan:start -->
## Accepted implementation proof plan

Every implementation-facing scenario requires the listed executable proof before implementation acceptance.

- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-001` / Community admin navigates to Community status from the Administrator area (test): Navigation test asserting the Administrator > Community menu includes a Community status entry that routes to the page for an authorized admin session (tasks.md Verification/Proof Plan, item 1).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-002` / Harvester is running (test): Component test asserting the page renders a Running badge/text, latest harvest timestamp, next harvest timestamp, and unresolved error count from the harvester status payload state (tasks.md Verification/Proof Plan, item 2).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-002` / Harvester is interrupted (test): Same component test (tasks.md Verification/Proof Plan, item 2) asserting the header renders an Interrupted badge/text distinctly from Running, alongside latest harvest, next harvest, and unresolved error count.
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-003` / Page loads with no prior selection (test): Component test rendering the page with unresolved errors present and asserting the detail pane and record list are empty with no error type marked selected on initial load (tasks.md Verification/Proof Plan, item 3).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-004` / Queue groups errors under their affected instance (test): Component test seeding errors across multiple instances and asserting the queue renders one group per affected instance with its errors nested underneath (tasks.md Verification/Proof Plan, item 4).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-005` / Applying a time range and content type filter updates the queue (test): E2E test applying a time range and content type filter and asserting queue group totals and available instances/error types update to match the filtered data set (tasks.md Verification/Proof Plan, item 5).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-006` / Selecting an instance reveals its error types, then selecting an error type shows detail (test): E2E test selecting an instance, then an error type, and asserting the detail pane shows description, fix guidance, and the matching affected-records list (tasks.md Verification/Proof Plan, item 6).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-007` / Detail pane shows a suggested action (test): Component test asserting the detail pane for a selected error type renders a suggested action element alongside description, fix guidance, and affected records (tasks.md Verification/Proof Plan, item 7).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-008` / Default description is plain language, raw exception text requires explicit expansion (test): Content/component test asserting error-type detail text matches curated plain-language copy and that raw exception/stack trace text is shown only behind an explicit support/debug expansion control (tasks.md Verification/Proof Plan, item 8).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-009` / Notify instance from the selected error type (test): E2E test that, with an error type selected, clicks Notify instance and asserts the notify flow opens scoped to that error type's affected instance (tasks.md Verification/Proof Plan, item 9).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-010` / Notify modal previews the task item content (test): E2E test opening the Notify instance modal and asserting the preview text includes the affected record count and the plain-language issue (tasks.md Verification/Proof Plan, item 10).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-011` / Task-sent indication appears without a page refresh (test): E2E test sending the notification task and asserting the selected error type shows Task sent / Sent just now without a page reload (tasks.md Verification/Proof Plan, item 11).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-012` / Record row shows ID below the title (test): Component/visual test asserting each record row renders the record ID on a line below the title element, not concatenated inline after the title text (tasks.md Verification/Proof Plan, item 12).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-013` / Layout stays aligned at common desktop widths (test): Responsive/visual regression test rendering the page at common desktop viewport widths and asserting header and content regions remain aligned with no horizontal overflow (tasks.md Verification/Proof Plan, item 13).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-014` / UI elements are built from Graphene/Pure components (test): Code review checklist plus component test asserting buttons, badges/tags, modals, tables, navigation, and task-item elements are built from Graphene/Pure component imports, with any prototype-only departure documented in design.md (tasks.md Verification/Proof Plan, item 14).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-015` / Page reuses fields already present on the status/error payload (deterministic-command): PR-blocking lint/code-review gate (tasks.md task 5.1, new '## 5. Guard Rails' section) that rejects any diff adding a fetch/query in the community-status page module for a field already declared in the documented payload contract (runningState, latestHarvestAt, nextHarvestAt, unresolvedErrorCount, affectedInstance, errorType, errorCount, affectedRecords, plainLanguageGuidance). Committed as a tracked task today, explicitly independent of Task 1.2; confirming the real endpoint only updates the gate's field-name mapping, not whether the gate exists (tasks.md Verification/Proof Plan, item 15).
- `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-016` / A missing dependency is recorded before implementation begins (deterministic-command): Process check confirming the Dependencies Before Implementation / Open Questions sections in proposal.md and design.md are reviewed and updated to record any unlocated dependency, gated on completion status of Tasks 1.1-1.5 (tasks.md Verification/Proof Plan, item 16).
<!-- agent-platform:intent-proof-plan:end -->
