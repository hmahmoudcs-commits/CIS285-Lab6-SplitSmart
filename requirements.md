# SplitSmart Requirements

## Authority and Scope

[UserRequirement.md](UserRequirement.md) is the original customer request. [The clarified feature specification](specs/001-splitsmart/spec.md) records later clarifications and acceptance detail. Customer requirements below remain the product baseline; clarifications and engineering choices are identified separately and do not silently replace that baseline.

## Original Customer Requirements

| ID | Feature | Customer requirement |
| --- | --- | --- |
| FR-001 | User accounts | Each person has an account to log in and access their own expense and balance information. |
| FR-002 | Group management | Users can create and manage groups (for example, roommates, trips, or parties) to track shared expenses and balances. |
| FR-003 | Expense details and receipts | Users can create expenses with an amount, date, description, and optional receipt image. |
| FR-004 | Participants and split methods | Users identify who was involved in an expense and split it equally or by percentage so balances can be adjusted appropriately. |
| FR-005 | Notifications | Users can be notified when other group members create an expense. |
| FR-006 | Approval workflow | Expenses are reviewed and approved before entering the system and affecting balances; a group may use a single review or multiple approval levels. |
| FR-007 | Balance tracking | The system tracks what each user owes every other user, based on expenses entered and subject to the approval workflow in FR-006. |
| FR-008 | Payment tracking | Users can record payments made to settle amounts owed so balances remain accurate. |
| FR-009 | Reporting | Users can view expense, balance, and payment reports or summaries, with custom date ranges and other parameters. |
| FR-010 | Supported platforms | The product is primarily web-based and is also available as Android and iOS apps. |

## Confirmed Clarifications

These eight decisions were confirmed after the original request; they are clarifications, not verbatim customer requirements from `UserRequirement.md`:

1. Each expense has exactly one payer, who may be outside the participant set.
2. Group membership is invitation-only, and group managers control membership.
3. Split remainders use the largest-remainder method.
4. Expense amounts must be strictly positive; zero and negative amounts are invalid.
5. Percentage shares must total exactly 100%.
6. Account creation and login use self-service email/password credentials.
7. Every other group member receives an in-app notification when an expense is submitted.
8. Users can record partial payments, and payment history is retained.

## Implementation Decisions

The following are engineering constraints from the project constitution, not customer-stated features:

- Use Java 21 and Maven for Java components and builds.
- Use Java Swing for the desktop MVP only; separate GUI presentation from business logic and use cohesive object-oriented domain models.
- Use `BigDecimal` for money, with documented currency precision, rounding, and remainder handling.
- Use JUnit 5 for Java tests and PlantUML for useful, maintained architecture or interaction diagrams.
- Maintain requirement-to-test traceability and require human review of AI-generated artifacts before acceptance.

The Swing MVP is a prototype subset, not the complete product. It may defer receipt images, notifications, multi-level approvals, and advanced report filters. It does not deliver FR-010. The web-first product and Android and iOS apps remain required and are not waived by desktop implementation choices.

## Remaining Open Questions

- **OQ-001 Accounts:** Email verification, credential recovery, and account closure.
- **OQ-002 Groups and roles:** Invitation details, manager permissions, and handling outstanding balances when a member is removed.
- **OQ-003 Currency and rounding:** Supported currencies and precision, percentage input precision, and deterministic ties in largest-remainder allocation.
- **OQ-004 Expense lifecycle:** Whether and how expenses can be edited or deleted after submission or approval.
- **OQ-005 Approval policy:** Approver selection, configurable levels, self-approval, rejection, and resubmission.
- **OQ-007 Payments:** Manual recording versus payment-provider integration; corrections, reversals, overpayments, disputes, and payment evidence.
- **OQ-008 Receipts:** Supported image types, size limits, access, and retention.
- **OQ-009 Reports:** Additional filters, grouping, export formats, and time-zone boundaries.
- **OQ-010 Platforms:** Supported browsers and OS versions, app distribution, and cross-platform parity expectations.
- **OQ-011 Quality targets:** Measurable performance, availability, accessibility, security, backup, and recovery targets.
- **OQ-012 Product architecture:** Web, Android, and iOS client and shared-service technologies that preserve common business rules.