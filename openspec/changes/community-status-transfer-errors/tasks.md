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

<!-- agent-platform:accepted-implementation-plan:start -->
## Accepted implementation execution plan

This managed section reflects the accepted internal enterprise implementation plan back into the canonical OpenSpec `tasks.md` file so execution order, slice boundaries, and required work remain visible in the portable source of truth.

Change: `community-status-transfer-errors`

Implementation delivery: `implementation-pr-required`

### 1. tasks.md: Repository Scope Note + Stale Checkbox Reconciliation (`tasks-md-scope-and-checkbox-reconciliation`)
- Assigned team: `community-status-test8`
- Task refs: `tasks.md:validate-production-dependencies`, `tasks.md:1.1`, `tasks.md:1.2`, `tasks.md:1.3`, `tasks.md:1.4`, `tasks.md:1.5`, `tasks.md:author-the-intent-package`, `tasks.md:2.1`, `tasks.md:2.2`, `tasks.md:2.3`, `tasks.md:define-the-delta-spec`, `tasks.md:3.1`, `tasks.md:3.2`, `tasks.md:3.3`, `tasks.md:3.4`, `tasks.md:3.5`, `tasks.md:3.6`, `tasks.md:3.7`, `tasks.md:3.8`, `tasks.md:3.9`, `tasks.md:3.10`, `tasks.md:3.11`, `tasks.md:3.12`, `tasks.md:3.13`, `tasks.md:3.14`, `tasks.md:3.15`, `tasks.md:3.16`, `tasks.md:prepare-review-readiness`, `tasks.md:4.1`, `tasks.md:4.2`, `tasks.md:4.3`, `tasks.md:4.4`, `tasks.md:4.5`, `tasks.md:4.6`, `tasks.md:4.7`, `tasks.md:4.8`, `tasks.md:4.9`, `tasks.md:4.10`, `tasks.md:4.11`, `tasks.md:4.12`, `tasks.md:4.13`, `tasks.md:4.14`, `tasks.md:guard-rails`, `tasks.md:5.1`

#### Execution scope
Make one narrowly scoped edit to `openspec/changes/community-status-transfer-errors/tasks.md`. No other file changes anywhere in this slice.

Do, in this exact order:

1. Insert a new section titled `## Repository Execution Scope` immediately after the `# Tasks: community-status-transfer-errors` title line and before `## 1. Validate Production Dependencies`. Its content must state:
   - This repository contains only `docs/` and `openspec/` -- no Pure/Graphene application source tree exists here.
   - Task Group 1 (Validate Production Dependencies, 1.1-1.5), Task 5.1 (Guard Rails), and the implementation-heavy portions of Test Scenarios, Verification/Proof Plan, and Accepted implementation proof plan (covering REQ-001 through REQ-015) require real Pure/Graphene UI code, wired backend APIs, and e2e/accessibility/responsive-visual/CI test suites that only exist in the target Pure product repository -- that work executes in that repository's own implementation-PR track, not this one.
   - This repository's implementation-PR track covers only: (a) reconciling stale `tasks.md` checkboxes for Task Groups 2-4 against already-approved, already-merged intent content, and (b) recording dependency-validation and guard-rail execution status as enterprise runtime state under `.agent-platform/enterprise/changes/community-status-transfer-errors/` (never under `docs/` or `openspec/`), which supports REQ-016 (documenting any missing production API/data dependency before implementation proceeds).
   - No other portion of this task list is staged for execution as a file change inside this repository.

2. Flip the following checkboxes from `- [ ]` to `- [x]` -- and change nothing else on those lines: 2.1, 2.2, 2.3, 3.1, 3.2, 3.3, 3.4, 3.5, 3.6, 3.7, 3.8, 3.9, 3.10, 3.11, 3.12, 3.13, 3.14, 3.15, 3.16, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 4.7, 4.8, 4.9, 4.10, 4.11, 4.12, 4.13, 4.14. Rationale: `proposal.md`, `design.md`, `tasks.md` itself, and `specs/community-status-transfer-errors/spec.md` already carry this exact concrete content, already reviewed and merged into `main` via the intent PR (#2); the checkbox state is stale, not the content. This is a checkbox-state correction only -- do not rewrite, reword, or renumber any task line.

3. Do NOT flip 1.1-1.5 or 5.1 -- they remain legitimately open/out-of-scope-for-this-repository, not resolved. Do NOT alter Task 5.1's committed wording (per plan-review guidance, that gate stays as written). Do NOT touch the Test Scenarios, Verification/Proof Plan, or Accepted implementation proof plan sections' content or checkboxes -- reference them only from the new Repository Execution Scope section.

Sizing: a single markdown insertion plus 33 single-character checkbox flips in one file -- comfortably fits one coder invocation.

Validation: run `git diff HEAD -- openspec/changes/community-status-transfer-errors/tasks.md` and confirm the only changes are (a) the new `## Repository Execution Scope` section and (b) the 33 checkbox flips listed above -- no other line differs. Also run `git diff HEAD -- .agent-platform/enterprise/changes/community-status-transfer-errors/` and confirm it is empty for this slice (this slice touches only the openspec path). Do not use `git status --porcelain` (this repo may contain unrelated untracked files).

References: `tasks.md` sections 1 (note only), 2.1-2.3, 3.1-3.16, 4.1-4.14, 5.1 (note only); `.intent-approved.json` (runtime state recording the merged intent PR -- cite as evidence, do not treat as an OpenSpec artifact and do not place any new runtime state under `openspec/changes/...`).

### 2. Runtime-State Record: Dependency Validation And Guard-Rail Execution Status (`dependency-and-guardrail-status-record`)
- Assigned team: `community-status-test8`
- Requirement IDs: `REQ-COMMUNITY-STATUS-TRANSFER-ERRORS-016`
- Task refs: `tasks.md:validate-production-dependencies`, `tasks.md:1.1`, `tasks.md:1.2`, `tasks.md:1.3`, `tasks.md:1.4`, `tasks.md:1.5`, `tasks.md:guard-rails`, `tasks.md:5.1`, `tasks.md:verification-proof-plan`, `tasks.md:accepted-implementation-proof-plan`

#### Execution scope
Create exactly one new enterprise runtime-state file: `.agent-platform/enterprise/changes/community-status-transfer-errors/implementation-slices/dependency-validation-status.md`. No other file changes anywhere in this slice. Do not create this file under `docs/` or `openspec/`, and do not let it appear in the eventual implementation PR diff.

Content to record:
1. Task Group 1 status (1.1-1.5, `tasks.md` 'Validate Production Dependencies'): cannot be validated from inside this repository. Confirmed: this repository contains only `docs/` and `openspec/` -- no Pure/Graphene application source tree, so there is no real `Administrator > Community` navigation tree, harvester status/error API, task-creation/task-panel model, or Graphene component library present here to inspect. Real validation of 1.1-1.5 must happen in the target Pure product repository's own implementation-PR track before that repository's implementation work proceeds.
2. Task 5.1 status (`tasks.md` 'Guard Rails'): the PR-blocking lint/code-review gate must be added to the target Pure product repository's own CI/lint tooling, operating on that repository's community-status page module source. Not executable here; left as committed and non-deferred per plan-review guidance, tracked here only as blocked-on-target-repository.
3. A note that this file supplements, and does not replace, the already-approved `proposal.md` 'Dependencies Before Implementation' and `design.md` 'Dependencies And Validation' sections -- those already document the same five dependencies as open before implementation; this file is the implementation-side status record satisfying REQ-016 ('Any missing production API/data dependency is documented before implementation proceeds', `tasks.md` Verification/Proof Plan item 16 / Accepted implementation proof plan REQ-016).
4. An explicit statement that this file is enterprise runtime state only: it must never be described as an OpenSpec artifact, must never be placed under `openspec/changes/...`, and must be excluded from the implementation PR diff.

Sizing: a single short status-record markdown file -- comfortably fits one coder invocation.

Validation: run `git diff HEAD -- .agent-platform/enterprise/changes/community-status-transfer-errors/implementation-slices/dependency-validation-status.md` and confirm it shows only this new file being added. Also run `git diff HEAD -- openspec/changes/community-status-transfer-errors/tasks.md` and confirm it is empty for this slice (this slice touches only the runtime-state path). Do not use `git status --porcelain`.

References: `tasks.md` 1.1-1.5, 5.1, Verification/Proof Plan item 16, Accepted implementation proof plan REQ-016; `proposal.md` 'Dependencies Before Implementation'; `design.md` 'Dependencies And Validation'.

<!-- agent-platform:accepted-implementation-plan:end -->
