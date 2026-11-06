# Phase 5 — Final Verification and Project Health Assessment

**Mode:** Independent final assessment. Do not modify application code, tests, configuration, dependencies, schema, or migrations.

## Objective

Independently verify the current codebase after Audit, Testing, Debugging & Bug Fixing, and Regression Testing. Determine whether reported fixes are present and verified, whether relevant checks pass, what risks remain, and whether the project is ready for its intended next stage.

This phase is an assessment, not another implementation or cleanup phase.

## 1. Establish the final state

- Inspect working tree, branch, diffs, modified files, uncommitted changes, and recent changes where available.
- Review all available phase reports and reconcile them with the actual code.
- Confirm that changes reported as complete are present and that relevant verification evidence exists.
- Identify unresolved bugs, blocked checks, skipped tests, flaky results, baseline failures, and discrepancies between reports and code.
- Inspect relevant scripts, config, dependencies, environment requirements, schema/migrations, API contracts, and deployment configuration as appropriate.
- Preserve all user changes and test data.
- If previous phase reports are missing, continue with available evidence but identify what cannot be independently confirmed.

Do not assume the current working tree matches a previous report.

## 2. Verify code and repository integrity

Inspect the actual diff for:

- Unintended changes, unrelated refactoring, unexpected files, and incomplete implementations.
- Temporary debugging code, abandoned workarounds, commented-out functionality, disabled tests, weakened assertions, and suppressed errors.
- Obvious integration mistakes, duplicate logic, dead code, and inconsistent types introduced by recent changes.
- Accidental secrets, credentials, private data, or sensitive configuration exposure.
- API, schema, migration, dependency, runtime, and configuration incompatibilities.
- Changes that could break existing callers, stored data, or expected application behavior.

Report suspicious evidence and risk. Do not automatically revert, reformat, or modify files.

## 3. Execute final quality checks

Inspect scripts/tooling and run relevant checks supported by the project, such as:

- Type checking, compilation, linting, and configured static analysis.
- Relevant unit/integration tests and the broadest feasible automated suite.
- End-to-end/browser tests for critical user journeys where supported.
- Production build where practical.
- API/database integration checks in a safe non-production environment.
- Migration consistency/schema compatibility checks when database changes exist.
- Security/authorization checks for critical access boundaries.
- Environment/configuration validation and deployment-related checks supported by repository evidence.

Targeted checks do not substitute for a broader suite. Record exact commands/procedures, results, evidence, and omissions. Use:

- `PASS`: Executed successfully and relevant checks passed.
- `FAIL`: Executed and produced an actionable failure.
- `BLOCKED`: Essential prerequisite unavailable.
- `NOT RUN`: Not executed; reason documented.

Do not claim deployment, production verification, live monitoring, or a successful migration unless actually performed and authorized. Do not access or modify production merely to complete this assessment.

## 4. Reconcile every important previous finding

For each finding, determine its current status:

- `FIXED AND VERIFIED`: Correction is present and supporting verification passed.
- `FIXED BUT UNVERIFIED`: A change is present but evidence is insufficient.
- `UNRESOLVED`: Original defect remains.
- `REGRESSION DETECTED`: A fix or recent change introduced a confirmed defect.
- `PRE-EXISTING FAILURE`: Baseline evidence shows the issue predated the relevant change.
- `NOT A BUG`: Investigation shows it was not a defect, with a defensible explanation.
- `BLOCKED`: Required evidence or infrastructure is unavailable.

Do not mark fixed just because code changed. Do not drop serious or difficult-to-reproduce findings. Preserve the distinctions between confirmed, high-confidence, potential, and environmental issues. Avoid double-counting duplicates.

## 5. Assess critical application boundaries

Based on implementation and executed evidence, assess:

### Application correctness

Core workflows, business logic, invalid inputs, async/concurrent operations, state consistency, and error/recovery paths.

### Security and access control

Authentication/session handling, authorization, roles, ownership, cross-user/workspace isolation, trust boundaries, and secrets exposure.

### Data integrity

Schema/type consistency, constraints, relationships, transactions, migrations, persistence, duplicate operations, partial failures, and concurrent writes.

### System integration

Frontend/backend contracts, API formats, shared modules, external services, environment assumptions, and runtime/dependency compatibility.

For each area, state whether evidence is adequate, partial, or absent. Missing verification is uncertainty, not proof of a defect.

## 6. Assess deployment and environment readiness

Where relevant:

- Check required environment variable names and configuration references for consistency without displaying secret values.
- Identify missing required configuration by key name only.
- Compare documented build, startup, migration, and deployment procedures with the implementation.
- Identify dependency/runtime incompatibilities and unsafe environment assumptions.
- Distinguish a local successful build from a verified deployment.
- Assess migration/deployment readiness only using safe available evidence.

Do not deploy, run production migrations, rotate credentials, modify live infrastructure, or perform destructive operations in this phase.

## 7. Assess test coverage and confidence

Distinguish:

- Tests that passed and exercised meaningful behavior.
- Tests that passed but provide limited protection against defects.
- Untested features and critical workflows.
- Checks blocked by unavailable infrastructure.
- Flaky/inconsistent results and manual checks that are not reproducible.
- Security-sensitive behavior not verified.
- Potential defects needing further investigation.

Use coverage percentages only if an actual report was generated and inspected. Passing tests do not establish that the application is bug-free.

## 8. Safety and scope

- Do not implement bug fixes or modify production source code.
- Do not change configuration, dependencies, schemas, migrations, or test expectations.
- Do not perform unrelated refactoring or feature development.
- Do not delete user data, reset databases, or run destructive operations against production.
- Do not overwrite, discard, or revert user changes.
- Do not weaken/disable tests or expose secrets.
- If a new defect appears, document evidence and recommend action without fixing it.
- If a critical security or data-integrity issue is found, flag it prominently and explain why the current state may be unsafe for its intended use.

## 9. Required final report

### A. Executive summary

Summarize current condition, strongest supporting evidence, key risks, assessed scope, and readiness for the intended next stage.

### B. Final checks and results

For each: exact command/procedure, status, evidence, scope, and limitations. Separate executed checks from blocked/skipped checks.

### C. Final findings

Separate:

- `CONFIRMED BUG`
- `HIGH-CONFIDENCE BUG`
- `POTENTIAL BUG`
- `ENVIRONMENT / INFRASTRUCTURE ISSUE`

For each actionable item: ID, severity, path/location, evidence, expected behavior, actual behavior or uncertainty, reproduction scenario, root cause if known, impact, and recommended action.

### D. Previous findings reconciliation

Count unique findings: fixed and verified, fixed but unverified, unresolved, confirmed regressions, potential issues, not bugs, and blocked. Explain discrepancies with earlier reports. Avoid double-counting related findings.

### E. Code and change integrity

Summarize changed files, whether the final diff was inspected, suspicious modifications, temporary code/disabled tests/incomplete work, compatibility implications, and uncommitted changes if observable. Do not claim commits or pushes that were not verified.

### F. Highest-risk areas

Identify the three highest-risk areas based on impact, evidence, likelihood, and verification gaps. Explain risk, known evidence, missing verification, and next action.

### G. Top three actions

Rank the most important reliability/readiness actions. Prioritize critical/high defects, regressions, security/data integrity, and essential blocked checks over cosmetic improvements. If no action is necessary in the assessed scope, say so and state limitations.

### H. Final project status

Choose exactly one:

- `HEALTHY`: Relevant checks passed, critical functionality has adequate verification, no known unresolved high-impact defects or confirmed regressions remain in the assessed scope, and no material verification gap prevents the intended next step.
- `NEEDS ATTENTION`: Unresolved defects, meaningful verification gaps, incomplete fixes, or non-critical failures require action.
- `CRITICAL`: A confirmed critical defect, serious security/data-integrity vulnerability, major regression, or evidenced severe issue makes the project unsafe or fundamentally unreliable for its intended use.

Missing evidence alone does not prove a critical defect. If checks are blocked, choose the status best supported by available evidence and explicitly lower confidence. State the assessed scope.

**Stop here.** Do not start another fix cycle or unrelated implementation work. Preserve findings and recommended actions for the next development decision.
