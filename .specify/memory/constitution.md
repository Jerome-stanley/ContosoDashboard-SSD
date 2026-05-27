<!--
Sync Impact Report
Version change: none -> 1.0.0
Modified principles: template placeholders -> Offline-First Training Runtime; Security and User Isolation by Default; Layered, Interface-Driven Architecture; Testable Vertical Slices; Explicit Change Management
Added sections: Technical Baseline; Delivery Standards
Removed sections: template placeholder section names
Templates requiring updates: ✅ .specify/templates/plan-template.md (reviewed, no content changes required)
Templates requiring updates: ✅ .specify/templates/spec-template.md (reviewed, no content changes required)
Templates requiring updates: ✅ .specify/templates/tasks-template.md (reviewed, no content changes required)
Templates requiring updates: ✅ .specify/templates/agent-file-template.md (reviewed, no content changes required)
Commands folder: none present under .specify/templates/commands/
Deferred items: TODO(RATIFICATION_DATE): original adoption date not recorded.
-->

# ContosoDashboard Constitution

## Core Principles

### I. Offline-First Training Runtime
ContosoDashboard MUST run end-to-end without cloud services, external APIs, or internet access.
Local SQL Server, the local filesystem, and in-process services are the default runtime.
If a feature needs a future cloud path, the cloud implementation MUST stay behind an
interface and MUST NOT change the default training flow.

Rationale: the repository is a training environment, so every baseline scenario must be
repeatable, inexpensive, and available offline.

### II. Security and User Isolation by Default
All reads and writes for user data MUST be authorized in the service layer before the
operation completes. UI pages and components MAY present data, but they MUST NOT be the
only security boundary. Every data access path MUST respect the current user's role,
ownership, and membership rules. Training authentication MAY remain mock-based, but it
MUST still enforce authorization checks and IDOR protection.

Rationale: the application handles per-user and per-project data, so authorization must be
checked where the data is actually accessed.

### III. Layered, Interface-Driven Architecture
Presentation, business logic, data access, and infrastructure concerns MUST stay separated.
Business rules MUST live in services or models, not in Razor pages or controllers.
File storage, notification delivery, authentication adapters, and other external concerns
MUST be accessed through interfaces so implementations can be swapped without rewriting
feature logic.

Rationale: the codebase stays testable and replaceable when infrastructure changes.

### IV. Testable Vertical Slices
Every meaningful feature MUST be deliverable as an independently testable slice with clear
acceptance criteria. New behavior MUST include the narrowest practical validation for the
touched layer, and user-visible changes SHOULD have scenario coverage or an executable
check that demonstrates the expected result.

Rationale: training work is easier to review and demo when each slice stands on its own.

### V. Explicit Change Management
Schema changes, seed-data changes, auth changes, and storage changes MUST document their
impact and compatibility assumptions before merge. Breaking changes MUST be called out,
versioned intentionally, and accompanied by the minimum supporting update to tests or docs.

Rationale: the repository is used as a teaching asset, so silent regressions are costly.

## Technical Baseline

- The application targets .NET 10.0 and uses ASP.NET Core Blazor Server.
- Data access MUST use Entity Framework Core with SQL Server LocalDB for local training.
- Authentication MUST default to the built-in mock cookie flow used by the app.
- Local filesystem storage and in-process services are the default for training features.
- Any cloud, identity-provider, or external-service integration MUST stay optional and
	behind abstractions.
- UI and styling changes MUST remain compatible with the existing Bootstrap-based layout.

## Delivery Standards

- New features MUST be written as independently testable user stories whenever possible.
- Security-sensitive changes MUST include negative-path validation for unauthorized access.
- Schema, seed, and configuration changes MUST include corresponding documentation updates
  when startup behavior or data shape changes.
- Validation MUST be performed with the narrowest executable check available for the slice
  being changed.
- Breaking changes MUST be surfaced explicitly in the plan or implementation notes before
  they are merged.

## Governance

This constitution supersedes informal project habits whenever the two conflict.
Amendments require a written justification, an updated version, and a revised
`Last Amended` date.

Versioning follows semantic rules: MAJOR for incompatible principle changes, MINOR for new
principles or materially expanded guidance, and PATCH for clarifications or wording
changes.

All plans, specs, tasks, and reviews MUST check proposed work against this constitution.
Any unresolved conflict MUST be documented before implementation begins.

Guidance that affects the running application SHOULD be reflected in the repository README
or equivalent runtime documentation.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE): original adoption date not recorded. | **Last Amended**: 2026-05-27
