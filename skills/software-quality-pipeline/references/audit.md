# Phase 1 — Codebase Audit

**Mode:** Investigation only. Do not modify source code, tests, configuration, dependencies, schemas, migrations, or generated project files.

## Objective

Audit the codebase as it exists now. Find defects and credible risk areas rather than reassuring the user that the code looks fine. Build an evidence-based map of the implementation, then systematically search for defects, especially around recent changes and high-impact application boundaries.

## 1. Establish the current development state

Follow the shared baseline protocol in the parent skill. In addition:

- Inspect recent commits and diffs when Git history is available, but do not assume the latest commit contains all current changes.
- Identify the actual architecture and runtime entry points before judging design or implementation.
- Inspect relevant project documentation, package scripts, dependencies, configuration, database schema/migrations, API routes, middleware, and existing tests.
- For monorepos, identify package boundaries and which commands apply to which package.
- Determine whether there are existing known failures or baseline test failures before attributing them to recent changes.

## 2. Build a focused architecture and dependency map

Summarize the relevant flow from user interaction to persisted effect, as applicable:

`UI/client → API/transport → auth/validation → business/domain logic → database/external service → response/state update`

Identify shared modules, ownership boundaries, trust boundaries, asynchronous processes, and high-coupling areas. Map recent changes to the modules and callers they can affect.

Do not create a large architecture document for its own sake. Include only enough detail to support findings and show coverage.

## 3. Hunt systematically for defects

Inspect relevant code and configurations for:

- Compile/type errors, lint issues, build failures, and invalid imports/exports.
- Runtime exceptions, unhandled rejections, null/undefined access, unsafe casts, and invalid assumptions.
- Incorrect asynchronous behavior, unawaited work, cancellation errors, race conditions, and concurrency hazards.
- State synchronization problems, stale caches, duplicate effects/requests, and incorrect loading/error/empty/success states.
- Incorrect business rules, state transitions, boundary conditions, duplicated or stale logic, and inconsistent sources of truth.
- Validation gaps, unsafe parsing, malformed input handling, and trust-boundary mistakes.
- API contract mismatches, incorrect status codes, response-shape drift, and frontend/backend boundary errors.
- Database query/schema issues, missing constraints, incorrect relations, transaction gaps, N+1 patterns, and unsafe persistence assumptions.
- Authentication/authorization flaws, missing ownership checks, role mistakes, and cross-user or cross-tenant data exposure.
- Error handling that hides failures, leaks sensitive information, or prevents recovery.
- Dependency, package-manager, build, runtime-version, environment, and configuration incompatibilities.
- Dead or unreachable code, abandoned paths, and feature flags that can hide defects.
- Regressions plausibly introduced by recent changes.

Use risk-based coverage. Spend more effort on authentication, money/transactions, data writes, ownership boundaries, migrations, critical workflows, and recently modified code than on cosmetic or isolated low-impact code.

## 4. Execute appropriate static and runtime checks

Inspect scripts/configuration before selecting commands. Execute relevant available checks, such as:

- Type checking or compilation.
- Linting and configured static analysis.
- Existing focused tests around suspicious code paths.
- Production build where practical.
- Safe local runtime checks when the project can be started without production credentials or destructive actions.

Do not run the full test campaign as a substitute for the dedicated Testing phase. Audit checks are used to validate suspicious areas and establish baseline evidence.

For each check, provide the exact command, status, and relevant output. If a check is not available or cannot safely run, explain why.

## 5. Verify and report each finding

For each finding provide:

`ID → confidence → severity → file/location → evidence → expected behavior → actual behavior or technical explanation → root cause (if known) → reproduction scenario (if known) → impact/blast radius → recommended fix`

Use `CONFIRMED BUG`, `HIGH-CONFIDENCE BUG`, or `POTENTIAL BUG` exactly as defined in the parent skill. Do not call a code smell a bug unless you explain a concrete failure mode. Do not duplicate several symptoms that clearly share one root cause without explaining their relationship.

If you cannot reproduce a suspected issue, state the attempts made and the evidence still missing. A plausible risk can be reported as potential, but it must not be described as an actual failure.

## 6. Scope restrictions

- Do not fix bugs or edit code.
- Do not create or modify tests during this phase.
- Do not refactor, redesign, or perform unrelated cleanup.
- Do not install dependencies merely to make a check possible without authorization.
- Do not run destructive commands or use production services for write testing.
- Do not modify schema, migrations, deployment settings, secrets, or environment files.

## 7. Required audit report

### A. Executive summary

State the scope, architecture areas inspected, most important findings, important exclusions, and confidence limitations.

### B. Findings

Separate into:

- `CONFIRMED BUG`
- `HIGH-CONFIDENCE BUG`
- `POTENTIAL BUG`

For each: ID, severity, path/location, evidence, root cause or likely cause, reproduction scenario, impact, and recommended fix.

### C. Findings by severity

Count Critical, High, Medium, and Low findings, keeping confidence categories visible. Do not double-count duplicates.

### D. Checks executed

List exact commands/procedures, `PASS` / `FAIL` / `BLOCKED` / `NOT RUN`, and meaningful results. Do not state that tests passed if they were not run.

### E. Coverage map and highest-risk areas

List relevant areas reviewed, areas only partially reviewed, and areas not inspected. Identify the three highest-risk areas and explain why.

### F. Top three fixes to prioritize

Rank by impact, likelihood, confidence, and dependency/blast-radius considerations. Distinguish required fixes from optional improvements.

### G. Project status

Choose one:

- `HEALTHY`: No confirmed or high-confidence defects were found in the assessed scope and relevant checks provide reasonable evidence. State any limits.
- `NEEDS ATTENTION`: Actionable defects, meaningful potential risks, or important verification gaps remain.
- `CRITICAL`: Evidence shows a critical security/data-integrity issue or severe core failure.

A limited audit with insufficient evidence should not be labelled HEALTHY merely because no bug was found.

### H. Handoff to Testing

Provide prioritized test targets, reproduction scenarios, expected behavior, relevant paths, required environment/services, and any baseline failures to account for.

**Stop here.** Do not begin the Testing phase unless the user explicitly requested full-pipeline mode.
