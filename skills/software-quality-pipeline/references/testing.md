# Phase 2 — Systematic Application Testing

**Mode:** Behavioral verification. Do not modify production source code or application configuration. Test-only changes are narrowly allowed when necessary and must be reported.

## Objective

Verify actual application behavior against expectations grounded in requirements, documentation, established behavior, and explicit code contracts. Expose failures and meaningful test-coverage gaps. Do not assume that a successful build or passing existing test suite proves the application works.

## 1. Understand the current state

- Inspect architecture, package scripts, framework/test configuration, environment requirements, test suites, and project instructions.
- Review available Audit findings and recent relevant changes. If the audit report is missing, derive the plan from the current implementation and disclose the missing handoff.
- Identify core features, critical user journeys, API contracts, data models, state transitions, and authorization boundaries.
- Derive expected behavior from explicit requirements, docs, established application behavior, and contracts. Do not invent product requirements.
- Inspect the working tree and preserve existing user changes and test data.
- Determine which services, credentials, browsers, databases, queues, and test fixtures are genuinely available.

## 2. Create a risk-based test plan

Build a concise test matrix before execution. For each critical scenario, identify:

- Feature/workflow and importance.
- Expected behavior and its source.
- Relevant boundary or failure condition.
- Test level: unit, component, integration, API, database, browser/E2E, or manual procedure.
- Existing test or new test needed.
- Environment/prerequisites.
- Priority and reason.

Prioritize critical workflows, security/authorization, data writes, recent changes, Audit findings, asynchronous flows, and areas with little existing coverage. Avoid blindly testing every file equally.

## 3. Execute existing checks and tests

Inspect available scripts and framework configuration before choosing commands. Use existing project tools wherever possible. Run relevant checks supported by the project:

- Unit, component, integration, API, database, and end-to-end/browser tests.
- Type checking, compilation, linting, and configured static checks.
- Production build where practical.
- Relevant authorization/security tests.
- Local integration checks where the required non-production environment is available.

Run targeted tests first, then the broadest feasible test suite. Do not claim a suite passed when only a subset ran. Record exact commands and relevant output.

Classify every check:

- `PASS`: Executed successfully and relevant assertions passed.
- `FAIL`: Executed and an assertion, behavior, or check failed.
- `BLOCKED`: Could not execute because an essential prerequisite was unavailable.
- `NOT RUN`: Intentionally not executed; explain why.

When a check fails, investigate whether it reflects an application defect, test defect, flaky test, incorrect expectation, dependency/tooling problem, or environment issue. Do not silently skip failures.

## 4. Test application behavior systematically

Select applicable scenarios based on the actual codebase.

### Core functionality

- Main user journeys and business-critical workflows.
- Create/read/update/delete, search, filtering, sorting, and pagination where present.
- Business rules, state transitions, and interactions between dependent features.
- Expected behavior after refresh, navigation, session changes, or repeated use.

### Validation and edge cases

- Missing, null, undefined, empty, malformed, and unexpected inputs.
- Invalid identifiers, wrong types, boundary values, and oversized inputs.
- Duplicate submissions, repeated operations, and invalid transitions.
- Missing/deleted records and inconsistent related data.

### Async behavior and state management

- Slow, failed, cancelled, or out-of-order requests.
- Duplicate requests, concurrent updates, and race conditions.
- Stale frontend state, cache invalidation, and server-response synchronization.
- Loading, success, error, empty, retry, and recovery states.

### API and database

- Request/response contract consistency, status codes, and error shapes.
- Constraints, relationships, transactions, query behavior, and persistence.
- Missing records, duplicate data, partial failures, and concurrent writes.
- Pagination boundaries and data consistency.

### Authentication and authorization

- Unauthenticated requests and expired/invalid sessions.
- Access to resources outside the user's role, ownership, or workspace.
- Role restrictions, server-side authorization, and cross-user/tenant isolation.
- Confirm that backend authorization protects resources independently of frontend controls.

### Integrations and failure handling

- External service timeouts, unavailable dependencies, malformed responses, and retry behavior where safely testable.
- Partial failures and recovery paths.
- Ensure error handling does not silently corrupt data or expose secrets.

Do not execute destructive scenarios against production or shared services. Use isolated test data and a dedicated test environment.

## 5. Find and verify failures

For each unexpected result:

- Reproduce it with a repeatable test or documented steps where possible.
- Compare actual output/state with the expected behavior and cite the requirement or contract source.
- Record inputs, execution conditions, affected workflow, and relevant evidence.
- Determine whether it is consistent, intermittent, or environment-dependent.
- Distinguish a product defect from a flaky test, test defect, unsupported expectation, or infrastructure failure.

For non-reproducible suspected bugs, state exactly what was attempted and what evidence is missing. Do not upgrade a suspicion to confirmed without support.

## 6. Identify test-coverage gaps

Prioritize missing tests around:

- Critical business logic and user journeys.
- Authentication, authorization, and data isolation.
- Error handling, recovery, and partial failures.
- Database writes, transactions, and consistency.
- Complex asynchronous workflows and concurrency.
- Findings from Audit and code changed recently.

Where useful, propose concrete test cases. Adding or modifying test files is allowed only when necessary to verify an uncovered behavior, must be narrowly scoped, and must not alter production behavior or weaken existing assertions. Clearly list any test files changed. Do not install new dependencies without a clear need and authorization.

Code coverage percentages are supporting evidence only, not proof of correctness. Do not invent percentages or claim coverage reports exist unless generated and inspected.

## 7. Scope and safety restrictions

- Do not fix application bugs in this phase.
- Do not modify production source code, schemas, migrations, dependencies, or application configuration to work around failures.
- Do not weaken assertions, delete failing tests, suppress errors, or change expected results just to obtain green tests.
- Do not reset databases, delete user data, or run production write operations.
- Use isolated test data and local/test environments where possible.
- Do not expose secrets in logs or reports.
- If credentials, services, or infrastructure are unavailable, document the limitation rather than inventing a result.

## 8. Required testing report

### A. Overall result

Summarize what was tested, important failures, critical paths covered, and major limitations.

### B. Test execution results

For each check: exact command/procedure, status, evidence, scope, and limitations.

### C. Findings

Separate:

- `CONFIRMED BUG`
- `HIGH-CONFIDENCE BUG`
- `POTENTIAL BUG`

For every finding: ID, severity, affected path, test performed, expected result and source, actual result, reproduction steps, evidence, likely root cause if known, and recommended next action.

Do not classify infrastructure/test failures as application bugs without evidence. Avoid duplicate findings for the same root cause.

### D. Coverage and untested areas

List critical paths tested, gaps, scenarios not executed, reasons, and prerequisites needed for verification.

### E. Recommended next actions

Rank the three highest-priority defects or coverage gaps by impact, likelihood, evidence, and dependencies. Distinguish actual failures from proposed test improvements.

### F. Final testing status

Choose one:

- `PASS`: Relevant tests passed, no confirmed defects found in tested scope, and no critical verification gaps remain.
- `PASS WITH LIMITATIONS`: Executed checks passed, but meaningful functionality remains unverified.
- `FAIL`: One or more important tests exposed confirmed application defects.
- `BLOCKED`: Essential constraints prevented meaningful testing.

Explain the evidence and scope. Passing tests do not prove the whole application is bug-free.

### G. Handoff to Debugging & Bug Fixing

Provide prioritized findings, reproduction procedures, expected/actual behavior, root-cause clues, relevant code paths, test files, baseline failures, and environmental blockers. Identify which findings are ready for fixing and which need more investigation.

**Stop here.** Do not begin debugging or production-code changes unless the user explicitly requested full-pipeline mode.
