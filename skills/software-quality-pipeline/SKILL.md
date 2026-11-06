---
name: software-quality-pipeline
description: Runs a disciplined, evidence-based software quality workflow for an existing codebase: audit, behavioral testing, root-cause debugging and bug fixing, regression testing, and final verification. Use when investigating bugs, validating application behavior, fixing verified defects, checking regressions, or assessing project/release readiness.
---

# Software Quality Pipeline

A phased, evidence-driven workflow for finding defects, verifying real application behavior, applying targeted fixes, protecting against regressions, and assessing the final state of a codebase.

## Core operating principles

1. **Evidence over reassurance.** Investigate the implementation and execute relevant checks. Never infer that a project is healthy merely because it looks well structured, compiles, or has passing tests.
2. **Understand before acting.** Inspect the architecture, project conventions, current working tree, available tooling, and the specific behavior under review before choosing commands or changing files.
3. **Preserve the user's work.** Record the initial Git state when available. Never discard, overwrite, reset, stash, revert, or reformat unrelated user changes.
4. **Keep phases separate.** Each phase has a defined purpose, allowed actions, and stopping point. Do not silently advance to another phase.
5. **Verify claims.** Report only checks actually executed and findings supported by code evidence, runtime evidence, or a defensible technical explanation.
6. **Minimize change.** During bug fixing, make the smallest safe change that corrects the root cause. Avoid unrelated refactoring, feature work, dependency churn, and architecture redesign.
7. **Be explicit about uncertainty.** Distinguish defects from test defects, flaky behavior, environment limitations, unavailable services, and unverified hypotheses.
8. **Risk-based effort.** Prioritize security, data integrity, critical user journeys, high-impact changes, and likely failure paths. Do not spend equal effort on low-risk and high-risk code.

## When this skill applies

Use this skill for codebase audits, systematic application testing, debugging and targeted bug fixing, post-fix regression testing, and final project or release-readiness verification.

This is an application-quality workflow. It does not replace the separate `infrastructure-reliability-review` skill, which focuses on operational reliability, infrastructure capacity, CI/CD resilience, observability, and risks that may cause future incidents. The two skills may complement each other, but keep their findings distinct.

## Select the requested phase

First determine what the user actually requested. Load only the corresponding reference document unless a broader workflow was explicitly requested.

| User intent | Reference to load | Phase purpose |
|---|---|---|
| Audit, inspect for bugs, codebase review | `references/audit.md` | Find and classify defects without fixing them |
| Test the application, verify behavior, assess coverage | `references/testing.md` | Execute tests and expose behavioral failures without production-code fixes |
| Debug, fix bugs, resolve findings | `references/debugging.md` | Establish root causes and make targeted fixes |
| Regression test, verify fixes after changes | `references/regression-testing.md` | Verify fixes and detect unintended breakage |
| Final verification, project health, release readiness | `references/final-verification.md` | Independently assess the final codebase state |

If the requested phase is ambiguous, ask one concise question identifying the phase. Do not run all phases merely because the skill was invoked.

### Supported invocation patterns

- `/software-quality-pipeline audit`
- `/software-quality-pipeline testing`
- `/software-quality-pipeline debugging`
- `/software-quality-pipeline regression`
- `/software-quality-pipeline final-verification`
- `/software-quality-pipeline full-pipeline`

Natural-language requests are also valid. Treat these as examples, not mandatory syntax.

## Full-pipeline mode

Run all five phases only when the user explicitly requests the complete/end-to-end pipeline or clearly authorizes the full sequence.

The default sequence is:

1. Audit
2. Testing
3. Debugging & Bug Fixing
4. Regression Testing
5. Final Verification

At each boundary:

- Produce the required report for the completed phase.
- Preserve findings, test evidence, changed-file details, unresolved risks, and blockers in a concise handoff.
- Re-read the current working tree and verify that the prior phase's assumptions still apply before acting.
- Do not treat a prior report as unquestionable truth; validate relevant findings against the current code.
- Do not repeat completed checks without a reason. Re-run only when code, configuration, environment, or evidence has changed, or when verification requires it.
- In ordinary single-phase mode, stop and wait for the user's next instruction. In explicitly authorized full-pipeline mode, continue to the next phase unless a safety boundary, missing authorization, or essential blocker prevents it.

A phase report is not permission to make changes in a later phase. In particular, audit and testing are not bug-fixing authorization.

## Shared execution protocol

Apply these steps in every phase, adapting depth to the requested scope.

### 1. Establish the baseline

- Identify the repository root and relevant application(s)/packages in a monorepo.
- Inspect `git status --short --branch` and relevant diffs before any operation when Git is available.
- Note pre-existing modifications, untracked files, generated files, and user work that must remain untouched.
- Inspect project instructions and conventions such as `AGENTS.md`, `agent.md`, `README`, architecture documents, package scripts, and contribution guidance when present.
- Identify language/framework versions, package manager, test/build/lint commands, runtime requirements, and relevant environment variables by name only.
- Do not print, copy, or expose secret values. Never put credentials in reports, test files, command output, or generated artifacts.

### 2. Map the relevant system

Inspect only what is relevant, but follow dependencies far enough to understand the behavior end to end. Depending on the project, this may include:

- Frontend routes, components, state management, forms, and client/server boundaries.
- API routes, controllers, middleware, services, domain logic, and contracts.
- Database schema, migrations, ORM models, constraints, transactions, and access policies.
- Authentication, authorization, roles, ownership, and tenant/workspace isolation.
- Background jobs, queues, scheduled tasks, external integrations, and asynchronous flows.
- Build/test/lint configuration, dependency versions, deployment configuration, and environment assumptions.

Do not assume a file or feature exists. Discover the actual project structure first.

### 3. Choose checks deliberately

- Inspect package scripts, project configuration, and documented commands before executing anything.
- Prefer the project's existing package manager, test framework, linters, type checkers, and browser tooling.
- Use the narrowest useful check first, then expand based on risk and results.
- Do not install dependencies or introduce tools without a clear need and user authorization where installation changes the environment or lockfiles.
- Do not run production writes, destructive database commands, migrations against live data, deployments, credential changes, or load/stress tests against shared services.
- Use local, isolated, disposable, or explicitly designated test environments.

### 4. Capture evidence

For each meaningful command or procedure, record:

- Exact command or reproducible procedure.
- Execution status: `PASS`, `FAIL`, `BLOCKED`, or `NOT RUN`.
- Relevant output, assertion, stack trace, log excerpt, or observation (redacted where necessary).
- Environment and scope limitations.

A command starting successfully does not mean the application behavior passed. A passing build does not establish functional correctness. A test that was not run must never be reported as passed.

### 5. Classify findings consistently

Use confidence and severity as separate dimensions.

**Confidence classification**

- `CONFIRMED BUG`: Reproducible incorrect behavior, or reliable runtime/test evidence demonstrates a defect.
- `HIGH-CONFIDENCE BUG`: Strong implementation or runtime evidence supports a defect, but full reproduction is unavailable.
- `POTENTIAL BUG`: Plausible concern that requires additional investigation; do not present it as established fact.

For regression reporting, use the regression-specific classifications in `references/regression-testing.md`.

**Severity classification**

- `CRITICAL`: Severe security compromise, major data loss/corruption, or a core system failure that makes the application unsafe or unusable.
- `HIGH`: Major feature failure, significant authorization/data-isolation flaw, or substantial user/business impact.
- `MEDIUM`: Material but bounded functional issue with a workaround or limited blast radius.
- `LOW`: Minor defect with limited impact and no meaningful security/data-integrity consequence.

Explain severity using impact and affected scope. Do not assign severity solely from a file name or theoretical possibility.

### 6. Prioritize using risk and dependencies

Prioritize findings and tests using:

- Impact on users, business-critical workflows, availability, security, and data integrity.
- Likelihood and evidence strength.
- Exposure and affected population.
- Reproducibility and exploitability where relevant.
- Dependency relationships and potential blast radius.
- Whether the issue blocks other work or release readiness.

Do not invent numeric risk scores or probabilities. Use a numeric score only when the project already defines a defensible scoring model.

## Phase boundaries and permissions

| Phase | Production source changes | Test-only changes | Main stopping point |
|---|---:|---:|---|
| Audit | Not allowed | Not allowed | Findings report |
| Testing | Not allowed | Only when necessary and explicitly scoped | Testing report |
| Debugging | Allowed for verified, in-scope defects | Allowed | Targeted fix verification and handoff |
| Regression | Not allowed | Only narrowly necessary for verification | Regression report |
| Final verification | Not allowed | Not allowed | Final health/readiness report |

Never cross these boundaries to make a report look better. If a phase uncovers work owned by another phase, document it and hand it off.

## Handoff contract

At the end of each phase, provide a concise handoff with the following fields where applicable:

- Phase and date/context.
- Repository/branch and baseline working-tree state.
- Scope inspected and important exclusions.
- Findings with stable identifiers (for example `AUD-001`, `TST-001`, `FIX-001`, `REG-001`).
- Severity, confidence, affected paths, evidence, and reproduction steps.
- Commands/checks actually executed and their statuses.
- Files changed, if the phase permits changes, and why.
- Outstanding issues, blockers, and missing evidence.
- Recommended next phase and its highest-priority inputs.

Keep IDs stable across reports when referring to the same underlying issue. Merge duplicate symptoms only when evidence supports a shared root cause; otherwise keep them separate. Do not silently drop unresolved findings.

By default, deliver reports in the conversation. Create or update report files in the repository only when the user asks for persistent reports or the project already has an established reporting convention. Before writing a report file, check the destination and avoid overwriting existing work.

## Required behavior when context is missing

- If prior phase reports are not available, continue from the current code and available evidence. State that the handoff is missing and do not fabricate prior results.
- If the environment, credentials, database, browser, or external service is unavailable, mark dependent checks `BLOCKED` or `NOT RUN` and explain what would be required.
- If the codebase is too large for exhaustive inspection, prioritize critical paths and state the actual scope covered. Do not claim exhaustive coverage.
- If a safe fix requires a product decision, destructive action, contract change, or production access, stop before that action and explain the decision needed.

## Final rule

Be technically skeptical, precise, and practical. The purpose is not to maximize the number of findings or force every status to green. The purpose is to provide a trustworthy picture of the codebase, fix verified defects safely when authorized, and make remaining uncertainty visible.
