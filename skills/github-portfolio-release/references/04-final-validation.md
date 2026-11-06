# Phase 4 — Final validation

**Mode:** validation first. Make only narrow, low-risk fixes within the authorized scope. Route application logic defects to `software-quality-pipeline` rather than expanding this phase into a broad refactor.

## Objective

Establish whether the project is technically ready for a repository push and, separately, whether it is deployable to its intended host.

## Procedure

### A. Select the correct environment and commands

1. Read manifest, lockfile, scripts, runtime version files, CI configuration, and deployment settings.
2. Use the package manager indicated by the authoritative lockfile and existing project conventions. For example, use the equivalent frozen/clean install mode supported by that manager. Do not mix package managers or generate a second lockfile.
3. Do not silently upgrade dependencies or change runtime versions to make checks pass.
4. Avoid destructive changes to the user's environment. If a clean install requires deleting an existing dependency directory or changing lockfiles, explain the effect first. Preserve caches where practical.
5. Treat missing credentials/services as environment blockers, not automatically as application bugs.

### B. Run appropriate checks

Use available scripts and tooling, typically:

- Dependency install or lockfile consistency check.
- Typecheck and lint where configured.
- Unit/integration tests and relevant test suites.
- Production build.
- Static analysis or framework checks already configured.
- Development server smoke test and critical-route checks where feasible.
- Browser inspection for layout, navigation, key interactions, responsive breakpoints, missing assets, and runtime console errors when browser access is available.

Run checks in an efficient order: inexpensive static checks first, then tests/build, then runtime/browser checks. Do not report a check as passed if it was skipped or only inferred.

### C. Inspect release-relevant behavior

- Confirm critical pages/routes load and primary workflows can be exercised.
- Check forms, validation, loading/empty/error states, auth boundaries, and data-dependent pages when feasible.
- Confirm assets, fonts, and environment-variable references are valid.
- Check obvious accessibility basics: semantic structure, keyboard access for primary controls, focus visibility, labels, and contrast issues that can be observed.
- Check responsive behavior at representative mobile/tablet/desktop widths when browser tools allow.
- Review dependency bloat only for clear issues: obviously unused direct packages, duplicate major libraries, or unusually large assets. Do not remove or replace dependencies based on speculation.

### D. Fix or hand off

Allowed without a separate broad refactor: typo in a command, incorrect README command, small ignore-rule correction, or a clearly identified low-risk documentation/configuration issue within the requested scope.

For application defects, unclear architecture issues, auth/data integrity bugs, or non-trivial behavior changes, record the evidence and hand off to `software-quality-pipeline` (audit/testing/debugging/regression as appropriate). Do not quietly modify business logic.

After any authorized fix, rerun the directly affected checks and report both original and post-fix results.

### E. Separate readiness dimensions

Report independently:

- **Repository/push readiness:** no known accidental secret/private data, publish set reviewed, docs and ignore behavior acceptable.
- **Build readiness:** build/typecheck/lint/test outcomes as actually run.
- **Runtime readiness:** routes and workflows observed working or not tested.
- **Deployment readiness:** host config, required variable names/configuration, build settings, and infrastructure dependencies. This is not the same as a successful build.

## Result labels

Use `PASS`, `FAIL`, `BLOCKED`, or `NOT RUN` for each check. Overall status should be `READY`, `READY WITH RISKS`, `NEEDS WORK`, or `BLOCKED`.

## Required report

Include exact commands (without secret values), concise results, environment/tool limitations, observed routes/workflows, safe fixes made, remaining bugs, and separate readiness conclusions. Do not push or deploy in this phase.
