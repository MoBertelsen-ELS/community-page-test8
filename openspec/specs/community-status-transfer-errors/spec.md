# community-status-transfer-errors Specification

## Purpose
TBD - created by archiving change community-status-transfer-errors. Update Purpose after archive.
## Requirements
### Requirement: Community admin can open the Community status page from Administrator > Community
The system SHALL let a community admin open the `Community status` page from `Administrator > Community`.

#### Scenario: Community admin navigates to Community status from the Administrator area
- GIVEN a user with community admin (or otherwise authorized) permissions is in the Administrator area
- WHEN they navigate to `Administrator > Community` and select `Community status`
- THEN the `Community status` page opens and displays the harvester/status summary header

### Requirement: The page shows harvester running/interrupted state, latest harvest, next harvest, and unresolved transfer error count
The system SHALL show harvester running/interrupted state, latest harvest timestamp, next harvest timestamp, and the unresolved transfer error count on the page header.

#### Scenario: Harvester is running
- GIVEN the harvester status payload reports the harvester as running, with a latest harvest timestamp, a next harvest timestamp, and a non-zero unresolved transfer error count
- WHEN a community admin opens Community status
- THEN the header shows a "Running" state, the latest harvest timestamp, the next harvest timestamp, and the unresolved transfer error count

#### Scenario: Harvester is interrupted
- GIVEN the harvester status payload reports the harvester as interrupted
- WHEN a community admin opens Community status
- THEN the header communicates the interrupted state distinctly from the running state, alongside the latest harvest, next harvest, and unresolved transfer error count

### Requirement: The default state has no selected error and does not overwhelm the user with record data
The system SHALL default to no selected error and SHALL NOT show record-level data until an error type is selected.

#### Scenario: Page loads with no prior selection
- GIVEN a community admin opens Community status for the first time in a session, with unresolved errors present
- WHEN the page finishes loading
- THEN no instance or error type is marked as selected, and the detail pane and affected-records list remain empty until the admin chooses an item

### Requirement: The unresolved queue groups errors by affected instance by default
The system SHALL group the unresolved error queue by affected instance by default.

#### Scenario: Queue groups errors under their affected instance
- GIVEN unresolved transfer errors exist across multiple member instances
- WHEN the community admin views the unresolved queue in its default state
- THEN errors are grouped under their affected instance (not by error type or time), matching the master-detail layout in the prototype

### Requirement: A manager can filter unresolved errors by time range and content type
The system SHALL let a manager filter the unresolved queue by time range and content type, with the queue's totals and available items updating consistently with the applied filters.

#### Scenario: Applying a time range and content type filter updates the queue
- GIVEN the unresolved queue is showing errors from all instances with no filters applied
- WHEN the community admin selects a time range and a content type filter
- THEN the queue re-renders showing only unresolved errors, instance groupings, and totals that match both applied filters, without a full page reload

### Requirement: Selecting an instance and error type shows a plain-language description, how-to-fix guidance, and affected records
The system SHALL show a plain-language description, how-to-fix guidance, and affected records when a community admin selects an instance and then one of its error types.

#### Scenario: Selecting an instance reveals its error types, then selecting an error type shows detail
- GIVEN the unresolved queue is grouped by affected instance
- WHEN the community admin selects an instance and then selects one of that instance's unresolved error types
- THEN the detail pane displays a plain-language description of the error, how-to-fix guidance, and the list of affected records for that error type

### Requirement: The error-type detail pane includes a suggested action alongside the plain-language description, how-to-fix guidance, and affected records
The system SHALL include a suggested action in the error-type detail pane in addition to the plain-language description, how-to-fix guidance, and affected records.

#### Scenario: Detail pane shows a suggested action
- GIVEN an error type is selected and its detail pane is showing the description, fix guidance, and affected records
- WHEN the detail pane finishes rendering
- THEN it also shows a suggested action for that error type (for example, notifying the affected instance)

### Requirement: Error descriptions avoid raw developer exception language unless explicitly expanded for support/debug use
The system SHALL present error descriptions in plain language by default and SHALL only show raw developer exception language when explicitly expanded for support/debug use.

#### Scenario: Default description is plain language, raw exception text requires explicit expansion
- GIVEN an error type's detail pane is showing its plain-language description
- WHEN the community admin has not explicitly expanded a support/debug view
- THEN the description does not include raw stack traces or developer exception text, and that detail is only shown if the admin explicitly opts into a support/debug expansion

### Requirement: The manager can notify the affected instance from the selected error type
The system SHALL let the manager notify the affected instance directly from the selected error type.

#### Scenario: Notify instance from the selected error type
- GIVEN an error type is selected in the detail pane
- WHEN the community admin selects `Notify instance`
- THEN the system starts the notify flow scoped to the member instance affected by that error type

### Requirement: The notify modal previews the task item that will appear in the instance task panel
The system SHALL preview, in the notify modal, the task item that will appear in the affected instance's task panel.

#### Scenario: Notify modal previews the task item content
- GIVEN the community admin has selected `Notify instance` for a selected error type
- WHEN the `Notify instance` modal opens
- THEN it previews the task item text the instance will receive, including the affected record count and the plain-language issue (for example, "54 community records are missing required data")

### Requirement: After sending, the selected error type shows an indication that the task has been sent
The system SHALL indicate, on the selected error type, that the notification task has been sent, immediately after sending.

#### Scenario: Task-sent indication appears without a page refresh
- GIVEN the community admin has reviewed the task preview in the `Notify instance` modal
- WHEN they send the task and the modal closes
- THEN the selected error type shows a "Task sent" / "Sent just now" indication without requiring a page refresh

### Requirement: Record IDs are displayed below record titles, not trailing inline after titles
The system SHALL display a record's ID below its title, not trailing inline after the title.

#### Scenario: Record row shows ID below the title
- GIVEN the affected records list is showing for a selected error type
- WHEN a record row renders
- THEN the record ID appears on its own line below the record title, rather than appended inline after the title text

### Requirement: Header and content remain aligned across common desktop widths
The system SHALL keep the page header and content aligned, and free of horizontal overflow, across common desktop widths.

#### Scenario: Layout stays aligned at common desktop widths
- GIVEN the Community status page is open
- WHEN the browser viewport is resized across common desktop widths (for example, typical laptop widths)
- THEN the header and the master-detail content remain aligned with no horizontal overflow

### Requirement: The implementation uses Graphene/Pure components for buttons, badges/tags, modals, tables, navigation, and task-item rendering where available
The system SHALL implement buttons, badges/tags, modals, tables, navigation, and task-item rendering using Graphene/Pure components where available.

#### Scenario: UI elements are built from Graphene/Pure components
- GIVEN the Community status page implementation
- WHEN buttons, badges/tags, modals, tables, navigation, and task-item elements are rendered
- THEN they use existing Graphene/Pure components rather than one-off custom implementations, except for prototype-only visual departures that are called out before product implementation

### Requirement: The implementation does not create extra backend calls for fields already available in the harvester status/error payload
The system SHALL NOT create extra backend calls to fetch fields that are already available in the harvester status/error payload.

#### Scenario: Page reuses fields already present on the status/error payload
- GIVEN the harvester status/error payload already includes a field needed by the page (for example, latest harvest timestamp or unresolved error count)
- WHEN the page renders that field
- THEN it reads the field from the existing payload response rather than issuing an additional backend call to fetch it separately

### Requirement: Any missing production API/data dependency is documented before implementation proceeds
The system's intent package SHALL document any missing production API/data dependency before implementation proceeds.

#### Scenario: A missing dependency is recorded before implementation begins
- GIVEN validation of production dependencies (real navigation entry point, harvester status/error API, task creation route, permissions model) is underway
- WHEN a required API or data dependency cannot be located in the real product
- THEN it is recorded in the proposal's Dependencies Before Implementation / Open Questions before any implementation task proceeds

