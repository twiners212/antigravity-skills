# Phase 4 — Regression Testing

**Mode:** Post-fix verification only. Do not modify production code or implement fixes.

## Objective

Verify that each bug fix corrects the original failure and that existing behavior remains intact in affected and dependent areas. Detect regressions introduced by the fixes without silently repairing them.

## 1. Establish the post-fix state

- Inspect the current working tree, diffs, modified files, and recent changes.
- Review Audit, Testing, Debugging reports, outstanding issues, and known baseline failures.
- Identify each implemented fix, its original failure, expected behavior, touched modules, and likely blast radius.
- Map changed files to direct callers, shared modules, API routes, database operations, authentication boundaries, and workflows.
- Inspect existing/new tests and determine what they actually verify.
- Preserve user changes and test data. Do not overwrite, discard, reset, stash, or revert unrelated work.
- If baseline results or reports are unavailable, state the limitation and avoid claiming a failure is newly introduced without comparison evidence.

## 2. Build change-to-test traceability

Create a compact matrix before execution:

| Fix/change | Original failure | Direct test | Dependent workflow | Boundary/edge case | Status |
|---|---|---|---|---|---|

Every changed production path should have an identified verification target. Where no test exists, define the safest repeatable procedure. Prioritize by blast radius, not simply by number of changed lines.

## 3. Re-test every fix

For every fix reported as complete:

- Repeat the original reproduction procedure or run the test that previously exposed the defect.
- Verify the original incorrect behavior no longer occurs.
- Verify normal expected behavior still works.
- Test relevant boundaries and invalid inputs involved in the defect.
- Check error handling, data integrity, authorization, concurrency, and state transitions where relevant.
- Inspect whether the regression test would fail if the original bug returned.

A fix is not verified merely because code changed, the app starts, or an unrelated test passed. If the original failure cannot be reproduced or required infrastructure is unavailable, mark `BLOCKED` or `INCONCLUSIVE`, not `PASS`.

## 4. Test related behavior and blast radius

Select cases based on actual changes. Consider:

### Frontend and state

- Components using modified data/state and shared hooks/utilities.
- Navigation, forms, validation, loading/error/empty/success/retry states.
- Cache invalidation, stale state, duplicate requests, and out-of-order responses.

### Backend and API

- Existing API consumers, request/response contracts, status/error shapes, and middleware.
- Shared business logic, other routes/callers, authentication, authorization, ownership, and role restrictions.

### Database and persistence

- Relevant CRUD paths, relationships, constraints, transactions, and query behavior.
- Duplicate submissions, concurrent writes, partial failures, existing-data compatibility, and migration behavior if changed.

### Shared modules and workflows

- Other features using changed types, schemas, services, hooks, components, or utilities.
- Complete user journeys traversing the modified behavior.
- Adjacent operations that should continue working when a dependency fails.

Do not blindly execute irrelevant categories. Record why a case is in or out of scope.

## 5. Run the regression suite

Execute relevant checks:

- Original failing tests and new regression tests.
- Tests for modified modules and direct dependents.
- Unit, component, integration, and API tests.
- End-to-end/browser workflows where supported.
- Type checking, compilation, linting, configured static checks, and production build where practical.
- Database/integration checks where safe test infrastructure is available.

Run targeted checks first, then the broadest feasible existing suite. Compare results to the pre-fix baseline when available. For each check, report exact command/procedure and one status:

- `PASS`: Executed and relevant assertions passed.
- `FAIL`: Executed and exposed a reproducible failure or unmet assertion.
- `BLOCKED`: Essential prerequisite unavailable.
- `NOT RUN`: Omitted intentionally, with reason.

Distinguish newly introduced regressions from pre-existing failures, flaky tests, incorrect assertions, and environment problems. A temporal relationship to a code change is not enough to prove causation.

## 6. Investigate failures without fixing them

When a check fails:

- Reproduce and collect evidence.
- Identify affected behavior and the change plausibly associated with it.
- Compare against baseline evidence where possible.
- Classify it as a confirmed regression, unresolved original bug, pre-existing failure, test/environment issue, or potential regression.
- Record paths, inputs, expected/actual results, steps, and likely cause.
- Assess severity, blast radius, and whether the issue blocks release or further verification.

Do not change production code, weaken assertions, delete tests, disable checks, or alter expected results to hide failures. Narrowly scoped test-only additions/adjustments are allowed only when necessary to verify behavior and must be reported; do not change a test merely to force a pass.

## 7. Safety and scope

- Verification-only phase: no production-code fixes, refactoring, feature changes, schema/migration/dependency/configuration changes.
- Do not run destructive operations or writes against production services.
- Use isolated non-production data and environments.
- Do not overwrite, discard, or revert user changes.
- Do not install unnecessary dependencies or expose secrets.
- Do not claim broad coverage when meaningful areas remain untested.

## 8. Required regression report

### A. Overall result

State whether original bugs remain fixed, regressions were found, critical workflows passed, and important checks were blocked or omitted.

### B. Original fix verification

For each fix:

`fix ID → changed path → original reproduction/test → expected result → actual result → evidence → VERIFIED / FAILED / BLOCKED / INCONCLUSIVE`

### C. Regression findings

Separate:

- `CONFIRMED REGRESSION`: Evidence demonstrates a change introduced incorrect behavior or broke previously working behavior.
- `CONFIRMED UNRESOLVED BUG`: An original bug remains or has returned.
- `PRE-EXISTING FAILURE`: Evidence indicates the failure existed before the relevant fix and is not attributable to it.
- `POTENTIAL REGRESSION`: Plausible issue requiring further investigation.
- `TEST / ENVIRONMENT FAILURE`: Test defect, flaky behavior, or infrastructure problem that is not established as an application regression.

For each: ID, severity, affected path, evidence, expected/actual behavior, reproduction scenario, suspected cause, baseline comparison, and recommended next action.

### D. Test execution results

List all exact commands/procedures, statuses, results, and limitations. Separate targeted results from broad-suite results.

### E. Coverage and limitations

List critical paths retested, unverified areas, missing regression tests, blocked checks, flaky results, and required prerequisites.

### F. Remaining risks and next actions

Rank the three highest-priority remaining issues. State whether each requires another fix cycle, more reproduction/testing, environment remediation, or no action.

### G. Regression status

Choose one:

- `PASS`: Original fixes verified, relevant checks passed, and no confirmed regressions found in tested scope.
- `PASS WITH LIMITATIONS`: Tested areas pass, but meaningful workflows/checks remain unverified.
- `FAIL`: Confirmed regressions exist, important original bugs remain, or critical checks fail.
- `BLOCKED`: Essential constraints prevent meaningful verification.

Explain the decision with evidence and coverage limitations.

### H. Handoff to Final Verification

Provide final fix states, regression findings, full check results, unresolved findings, changed-file/test traceability, baseline failures, and blocked areas.

**Stop here.** Do not fix newly discovered issues or begin Final Verification unless the user explicitly requested full-pipeline mode.
