# SplitSmart Constitution

## Purpose and Authority

This constitution governs engineering decisions for SplitSmart. The customer baseline is [UserRequirement.md](../../UserRequirement.md); this constitution does not amend or replace it. When an implementation decision conflicts with a customer requirement, the conflict must be recorded and resolved through an explicit customer-approved change, not by treating the implementation choice as a revised requirement.

## Customer Requirements

The following are product requirements stated by the customer:

- People need accounts to log in and see their own expense and balance information.
- Users need to create and manage groups for shared activities and track expenses and balances in those groups.
- Expenses need an amount, date, description, split method (equal or percentage), involved users, and an optional receipt image.
- Group members need notification when another member creates an expense.
- Expenses must be reviewed and approved before they affect balances; approval may be single-step or multi-level according to group needs.
- The system must track amounts owed between users from entered expenses and record settlement payments so balances remain accurate.
- Users need expense, balance, and payment reports, with custom date ranges and other parameters where supported.
- The product is web-first and must also be available on Android and iOS.

These requirements are the acceptance baseline. Any refinement must retain a trace to its source requirement and must not silently narrow its scope.

## Implementation Principles

### 1. Java 21 Baseline

Use Java 21 for Java components. Keep the selected language level and runtime requirements explicit and consistent across local builds and CI.

### 2. Maven Builds

Use Maven to manage Java dependencies, build lifecycle, and tests. Builds must be reproducible from the checked-in project configuration; avoid undocumented machine-specific setup.

### 3. Object-Oriented Domain Design

Represent core expense-sharing concepts and their rules with cohesive, well-encapsulated object-oriented models. Keep responsibilities focused and dependencies explicit; do not make the domain model depend on UI widgets.

### 4. Separate Presentation from Business Logic

Keep GUI event handling and presentation separate from domain rules, calculations, and persistence boundaries. Java Swing is the chosen GUI technology for a desktop client when one is in scope. A Swing client alone does not meet the customer's web-first and Android/iOS requirements; those platforms require suitable clients and shared service/domain contracts, or an explicitly approved change to the customer baseline.

### 5. Exact Money Arithmetic

Use `BigDecimal` for monetary amounts, allocations, and balance calculations. Define scale, rounding, and remainder-allocation rules explicitly at the relevant business boundary. Do not use binary floating-point types for money.

### 6. JUnit 5 Verification

Use JUnit 5 for Java automated tests. Test domain calculations and approval/balance behavior independently of Swing, and add focused integration tests where component boundaries or persistence behavior require them.

### 7. PlantUML Documentation

Use PlantUML for maintained architecture and interaction diagrams when a diagram clarifies component boundaries or important workflows. Keep diagrams aligned with the implementation and link them to the requirements or design decisions they explain.

### 8. Requirement Traceability

Give each implemented behavior a trace to a customer requirement or a clearly identified implementation decision. Maintain traceability through design, tests, and acceptance evidence. Flag gaps, conflicts, and deferred requirements rather than implying they are complete.

### 9. Human Review of AI Output

AI-generated code, tests, diagrams, and documentation are proposals, not accepted project decisions. A human must review them for correctness, security, privacy, requirement coverage, and consistency with this constitution before they are merged or treated as authoritative.

## Governance

Customer requirements define what the product must do; implementation decisions define how the team chooses to build it. Record implementation decisions separately from customer requirements and document the rationale and affected requirements when a decision constrains product scope. Amend this constitution only through explicit human review, and preserve the customer baseline unless an authorized requirement change is made.