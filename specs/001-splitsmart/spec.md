# SplitSmart Feature Specification

**Status:** Draft for customer clarification
**Authority:** [UserRequirement.md](../../UserRequirement.md) is the authoritative customer request.
**Engineering governance:** [SplitSmart Constitution](../../.specify/memory/constitution.md)

## 1. Purpose and Scope

SplitSmart helps individuals and groups record shared expenses, review them, track balances and settlements, and see reports. This specification converts the stated customer features into testable requirements while preserving the requested web-first, Android, and iOS product scope.

The customer request does not define detailed account lifecycle, authorization roles, currency rules, approval policy, notification channels, or platform-specific capabilities. Those details remain open questions below; this specification does not assume answers for them.

## 2. Actors

- **Account holder:** A person with a SplitSmart account who logs in and accesses their own expense and balance information.
- **Group manager:** An account holder who creates or manages a group. Specific management permissions are not yet defined.
- **Expense submitter:** A group member who records an expense and identifies the users involved.
- **Expense participant:** A user identified as involved in an expense.
- **Reviewer/approver:** A person authorized by the group to review an expense before it affects balances. The authorization policy is an open question.
- **Payment recorder:** A user who records a payment settling an amount owed.
- **Report viewer:** An account holder who views expense, balance, or payment reports.
- **Notification recipient:** A group member notified when another member creates an expense.

One person may act in several roles. The customer has not specified role separation or permission rules.

## 3. Customer Requirements Inventory

All items below remain in the full product scope. “Swing MVP” indicates only whether the item is included in the proposed desktop prototype; it does not change the customer requirement or final platform scope.

| ID | Customer feature | Preserved requirement | Swing MVP |
| --- | --- | --- | --- |
| FR-001 | User accounts | Each person has an account, can sign up and log in using email and password, and can access their own expense and balance information. | In scope |
| FR-002 | Group management | Users can create groups; group managers control membership through invitations. | In scope |
| FR-003 | Expense details and receipts | Users can create expenses with a positive amount, date, description, and an optional receipt image. | Core details in scope; receipt image deferred |
| FR-004 | Participants and split methods | Users identify who was involved, record one payer (who may be outside the participant set), and split the expense equally or by percentages totaling exactly 100%. | In scope |
| FR-005 | Notifications | All other group members receive an in-app notification when an expense is submitted. | Deferred |
| FR-006 | Approval workflow | Expenses are reviewed and approved before they are added and used to adjust balances; a group may use one or multiple approval levels. | In scope, single-level prototype policy |
| FR-007 | Balance tracking | The system tracks what each user owes every other user based on entered expenses. | In scope |
| FR-008 | Payment tracking | Users can record payments that settle amounts owed, and balances reflect those payments. | In scope |
| FR-009 | Reporting | Users can see expense, balance, and payment reports or summaries; custom date ranges and other parameters are supported. | Basic summaries in scope; custom filters deferred |
| FR-010 | Supported platforms | The product is primarily web-based and also has Android and iOS apps. | Not delivered by Swing prototype; remains full-product requirement |

## 4. User Scenarios

### US-001: Access personal information

An account holder signs up or logs in with email and password, then views their expense and balance information.

**Related requirements:** FR-001

### US-002: Organize a shared group

An account holder creates or manages a group for a shared activity so members can track expenses and balances together.

**Related requirements:** FR-002

### US-003: Record and allocate an expense

An expense submitter records a positive-amount expense, identifies the involved users, records one payer (who may or may not be a participant), selects an equal or percentage split, and optionally attaches a receipt. The system prepares the expense for review.

**Related requirements:** FR-003, FR-004, FR-006

### US-004: Review an expense

A reviewer inspects a submitted expense and approves or does not approve it. Only an approved expense is included in balances. Groups may require one or multiple review levels.

**Related requirements:** FR-006, FR-007

### US-005: See updates and settle balances

All other group members receive an in-app notification when an expense is submitted. Users review their bilateral balances and record full or partial payments as they settle amounts owed.

**Related requirements:** FR-005, FR-007, FR-008

### US-006: Review activity

An account holder reviews summaries of expenses, balances, and payments, optionally filtered by date range and other report parameters.

**Related requirements:** FR-009

### US-007: Use the product on supported platforms

A customer uses SplitSmart primarily through the web product and can also use Android and iOS apps.

**Related requirements:** FR-010

## 5. Functional Requirements and Acceptance Criteria

### FR-001: User accounts

The system shall provide an individual account for each person, support login, and make that account holder's expense and balance information accessible to them.

- **AC-001.1:** A person can create an account using email and password.
- **AC-001.2:** Given valid email and password for an existing account, when the person logs in, then the system identifies the account holder and displays their expense and balance information.
- **AC-001.3:** Given invalid login credentials, when a person attempts to log in, then the system does not grant access to account information.
- **AC-001.4:** Credential verification, recovery, and account closure behavior remain to be agreed; see OQ-001.

### FR-002: Group management

The system shall let users create and manage groups of people for shared expense and balance tracking.

- **AC-002.1:** Given an account holder, when they create a group, then that group is available for recording shared expenses.
- **AC-002.2:** Group membership is invitation-only; a person can join only through an invitation.
- **AC-002.3:** Only a group manager can issue invitations or otherwise change group membership.
- **AC-002.4:** The invitation flow and the effects of removing a member who has outstanding balances remain to be agreed; see OQ-002.

### FR-003: Expense details and optional receipt

The system shall let a user create an expense with an amount, date, description, and optional receipt image.

- **AC-003.1:** Given a valid expense submission, when it is saved, then its amount, date, and description are retained and shown to users authorized to see it.
- **AC-003.2:** Given an expense with a receipt image, when it is saved, then the receipt remains associated with that expense and can be retrieved by authorized users.
- **AC-003.3:** Given an expense without a receipt image, when it is saved, then it is accepted without requiring a receipt.
- **AC-003.4:** An expense amount must be greater than zero; zero and negative amounts are rejected with a validation message.
- **AC-003.5:** Supported image types, size limits, and receipt retention rules must be agreed; see OQ-008.

### FR-004: Participants and split methods

The system shall let the expense submitter identify the users involved, record exactly one payer separately from the participants, and select an equal split or a percentage split so the resulting allocations can be used in balance calculations after approval. The payer may be a participant or a non-participant.

- **AC-004.1:** Given an expense and its involved users, when the submitter selects an equal split, then the system calculates an allocation for each identified user.
- **AC-004.2:** Given an expense and its involved users, when the submitter supplies percentage shares totaling exactly 100%, then the system calculates an allocation for each identified user according to those shares.
- **AC-004.3:** Given percentage shares totaling less than or greater than 100%, when the submitter attempts submission, then the expense is rejected with a validation message.
- **AC-004.4:** An expense with invalid or incomplete payer, participant, or split data is not submitted for approval; the system identifies the data that needs correction.
- **AC-004.5:** Allocations use the largest-remainder method to reconcile to the expense total in the currency's smallest unit. Currency precision and deterministic tie-breaking for equal remainders remain to be agreed; see OQ-003.
- **AC-004.6:** The payer is recorded separately from participants; the payer's payment and participant allocations determine the resulting bilateral balances. The payer's own share does not create an amount owed to themselves.

### FR-005: Expense notifications

The system shall provide an in-app notification to every other member of the group when an expense is submitted.

- **AC-005.1:** Given an expense submitted in a group, when it is recorded, then every other group member can see an in-app notification about it.
- **AC-005.2:** The notification is made available at submission and does not wait for approval.

### FR-006: Expense approval workflow

The system shall support review and approval before an expense is added to the system's balance-affecting records. A group can use a single review step or multiple approval levels.

- **AC-006.1:** Given a newly submitted expense, when it has not completed the group's approval workflow, then it does not affect balances.
- **AC-006.2:** Given an expense that completes the required approval workflow, when approval is recorded, then it becomes eligible to affect balances.
- **AC-006.3:** Given an expense that is not approved, when the decision is recorded, then it does not affect balances.
- **AC-006.4:** The approver selection, number of levels, rejection and resubmission behavior, and whether an expense submitter may approve their own expense must be agreed; see OQ-005.

### FR-007: Bilateral balance tracking

The system shall track the amount owed by each user to every other user, taking approved expenses into account.

- **AC-007.1:** Given an approved expense, when its allocations are applied, then the relevant users' bilateral balances reflect the expense.
- **AC-007.2:** Given an expense that is pending or not approved, when balances are viewed, then that expense has not changed the balances.
- **AC-007.3:** Given a recorded settlement payment, when balances are recalculated, then the affected bilateral amount owed reflects the payment.
- **AC-007.4:** For an approved expense, the payer's recorded payment and the participant allocations produce bilateral amounts owed; a payer who is also a participant is not shown as owing money to themselves.
- **AC-007.5:** The sum of allocations for an approved expense reconciles to its amount using the largest-remainder method and the agreed currency precision; see OQ-003 for the remaining precision and tie-break decisions.

### FR-008: Payment tracking

The system shall let users record full or partial payments already made to settle amounts owed, retain payment history, and reflect those payments in balances.

- **AC-008.1:** Given a valid payment record, when it is saved, then the payment is associated with the relevant users and reflected in their balance.
- **AC-008.2:** Given a partial payment, when it is saved, then only the recorded amount is applied to the outstanding bilateral balance.
- **AC-008.3:** Given a payment, when a user reviews payment history, then the recorded payment is included in that user's payment information.
- **AC-008.4:** Payment correction or reversal, overpayment handling, and any payment evidence remain to be agreed; see OQ-007.

### FR-009: Reports and summaries

The system shall provide expense, balance, and payment reports or summaries. Users can request custom date ranges and other parameters.

- **AC-009.1:** An account holder can view an expense summary, a balance summary, and a payment summary for information they are authorized to access.
- **AC-009.2:** Given a selected date range, when a report is generated, then the report includes records within that range and excludes records outside it.
- **AC-009.3:** The additional report parameters and any export formats must be agreed; see OQ-009.

### FR-010: Web, Android, and iOS availability

The product shall be primarily web-based and shall also be available as an Android app and an iOS app.

- **AC-010.1:** The released product has a web experience identified as its primary client.
- **AC-010.2:** The released product has usable Android and iOS apps.
- **AC-010.3:** Core account, group, expense, approval, balance, payment, and reporting workflows are available on the supported product platforms; any platform differences require explicit customer agreement.
- **AC-010.4:** A Java Swing desktop prototype does not satisfy this requirement and cannot be represented as the completed product.
- **AC-010.5:** Supported OS versions, distribution channels, and detailed parity expectations must be agreed; see OQ-010.

## 6. Non-Functional Requirements

These requirements are derived from the constitution or are explicitly marked as inferred. They do not add customer features or replace the functional requirements above.

### NFR-001: Monetary correctness

Monetary amounts, allocations, payments, and balances shall use exact decimal arithmetic. The implementation shall use `BigDecimal` for Java monetary calculations and shall not use binary floating-point types for money.

- **AC-NFR-001.1:** Tests with decimal amounts demonstrate that calculations do not introduce binary floating-point approximation.
- **AC-NFR-001.2:** Equal and percentage allocations reconcile to the expense total using the largest-remainder method and a documented currency scale; the tie-break rule for equal remainders remains open under OQ-003.
- **AC-NFR-001.3:** Percentage allocations are accepted only when submitted shares total exactly 100%.
- **AC-NFR-001.4:** Pending and rejected expenses do not change balances; approved expenses and recorded payments do so according to their recorded amounts.

### NFR-002: Access protection (inferred from personal account information)

The system shall not expose personal expense or balance information to an unauthenticated person or to an account without authorization to access that information.

- **AC-NFR-002.1:** An unauthenticated request for protected account information is denied.
- **AC-NFR-002.2:** An account cannot access another account's personal information unless an explicitly defined group or product permission allows it.
- **AC-NFR-002.3:** Group-level visibility and authorization rules must be agreed; see OQ-002 and OQ-011.

### NFR-003: Separation and testability

Business rules and monetary calculations shall be independent of Swing presentation code. Java domain behavior shall be verifiable with JUnit 5 tests without requiring GUI interaction.

- **AC-NFR-003.1:** Automated tests exercise expense allocation, approval effects, balance changes, and payment effects without launching Swing.
- **AC-NFR-003.2:** Swing event handling delegates business operations rather than implementing balance or allocation rules in UI components.

### NFR-004: Traceability and human review

Implemented behavior and verification evidence shall trace to a customer requirement or a clearly identified implementation decision. AI-generated artifacts require human review before acceptance, as required by the constitution.

- **AC-NFR-004.1:** Each implemented FR has linked test or acceptance evidence, and unresolved requirements are recorded rather than marked complete.
- **AC-NFR-004.2:** AI-generated code, tests, diagrams, or documentation are not treated as accepted until a human has reviewed them.

No response-time, availability, scale, accessibility, backup, or recovery targets were supplied by the customer. Such targets require agreement before they can be given objective acceptance thresholds.

## 7. Java Swing MVP Subset

This is a proposed desktop prototype scope, not a customer-approved reduction of the full product. The complete requirements inventory in Section 3 remains in force.

### Included in the Swing MVP

- FR-001 email/password account creation and login, plus access to a prototype account's expense and balance information.
- FR-002 basic group creation and management needed for the prototype workflow.
- FR-003 positive expense amount, date, and description; receipt images are deferred from this prototype.
- FR-004 one payer recorded separately from participants, plus equal or exact-100%-percentage allocation using largest remainder.
- FR-006 a single-level approval flow for the prototype; the full product still supports a group-configurable single or multi-level workflow.
- FR-007 bilateral balance tracking for approved prototype expenses.
- FR-008 recording settlement payments and reflecting them in balances.
- FR-009 basic expense, balance, and payment summaries; custom date ranges and other report parameters are deferred from this prototype.

### Not delivered by the Swing MVP; still required for the full product

- FR-003 optional receipt-image handling.
- FR-005 notifications when group members create expenses.
- Multi-level and group-configurable approval behavior within FR-006.
- Custom report ranges and additional parameters within FR-009.
- FR-010 the web-first product and Android and iOS apps.

The Swing MVP is not platform acceptance for FR-010. Any claim that it is production-ready, or that its omissions are removed from scope, requires explicit customer approval and a documented requirement change.

## 8. Implementation Decisions from the Constitution

These are engineering constraints, not customer-stated product requirements:

- Use Java 21 for Java components and Maven for builds and dependency management.
- Use Java Swing for the desktop MVP only; keep presentation separate from domain rules, calculations, and persistence boundaries.
- Use cohesive, encapsulated object-oriented domain models.
- Use `BigDecimal` for money and document scale, rounding, and remainder-allocation rules.
- Use JUnit 5 for Java automated tests.
- Use PlantUML for maintained architecture or interaction diagrams where they clarify boundaries or workflows.
- Maintain traceability from implemented behavior to requirements or identified decisions.
- Require human review of AI-generated code, tests, diagrams, and documentation before acceptance.

These decisions do not waive or alter the web, Android, or iOS requirements. The architecture and technology choices for those clients and shared services remain open until decided.

## 9. Clarification Record

### Confirmed Decisions

The following decisions were provided in response to the clarification questions and are now reflected in the requirements above:
	- Each expense has exactly one payer, who may be outside the participant set.
	- Group membership is invitation-only; group managers control membership.
	- Split remainders use the largest-remainder method.
	- Expense amounts must be strictly positive; zero and negative amounts are invalid.
	- Percentage shares must total exactly 100%.
	- Account creation and login use self-service email/password credentials.
	- Every other group member receives an in-app notification when an expense is submitted.
	- Users can record partial payments, and payment history is retained.

### Remaining Open Questions

- **OQ-001 Accounts:** How are email addresses verified, and how do credential recovery and account closure work?
- **OQ-002 Groups and roles:** What is the invitation flow, can invitees accept before creating accounts, what other manager permissions exist, and what happens to outstanding balances when a member is removed?
- **OQ-003 Currency and rounding:** Which currencies and currency precision are supported? What deterministic tie-break is used when largest-remainder shares have equal remainders, and what percentage input precision is allowed?
- **OQ-004 Expense lifecycle:** Can expenses be edited or deleted after submission or approval, and how are corrections represented?
- **OQ-005 Approval policy:** Who approves, how many levels can a group configure, can submitters approve their own expenses, and how do rejection, edits, and resubmission work?
- **OQ-007 Payments:** Are payments manually recorded only or should external payment processing be integrated? How are mistakes, reversals, overpayments, disputes, and payment evidence handled?
- **OQ-008 Receipts:** Which image formats and size limits are supported, who can view receipts, and how long are they retained?
- **OQ-009 Reports:** Which additional filters, grouping options, and export formats are required? Which time zone determines date-range boundaries?
- **OQ-010 Platforms:** Which browser and OS versions are supported, how are Android/iOS apps distributed, and what platform parity is expected at launch?
- **OQ-011 Quality targets:** What measurable targets are required for performance, availability, accessibility, security, backup, and recovery?
- **OQ-012 Product architecture:** What services or client technologies will deliver the web-first, Android, and iOS product while sharing business rules with the Java domain? Java Swing alone cannot satisfy these targets.

## 10. Traceability Rule

FR-001 through FR-010 preserve the customer feature inventory from [UserRequirement.md](../../UserRequirement.md). NFR IDs identify constitution-derived or explicitly inferred quality constraints. An implementation or test that cannot be traced to one of these requirements or to a separately recorded implementation decision must not be presented as customer-requested scope.