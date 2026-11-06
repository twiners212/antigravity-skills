# Phase 3 — Debugging & Bug Fixing

**Mode:** Root-cause investigation and targeted implementation changes for verified, in-scope defects.

## Objective

Investigate findings from Audit and Testing, verify that they still apply, identify underlying causes, and make the smallest safe changes that correct verified bugs without introducing regressions or unrelated scope changes.

## 1. Establish the current state

- Inspect the working tree, existing diffs, recent commits where available, relevant documentation, and current implementation.
- Review Audit findings, Testing reports, failing tests, runtime evidence, and reproduction steps.
- If prior reports are missing, inspect the relevant code and reproduce/verify suspected problems before changing anything.
- Recheck architecture, dependencies, configuration, schema, migrations, API contracts, callers, and relevant test coverage.
- Preserve pre-existing user changes. Never overwrite, discard, reset, stash, or revert unrelated work.
- Reclassify each finding as confirmed, high-confidence, potential, duplicate, already resolved, not a bug, or environment-related.

## 2. Prioritize and investigate

Prioritize using severity, security/data-integrity implications, user impact, likelihood, dependencies, and blast radius.

For each actionable finding:

1. Reproduce it when practical and capture a reliable baseline.
2. Trace the relevant path across frontend, API, middleware, domain logic, database, queues, and integrations as applicable.
3. Inspect inputs, outputs, state transitions, asynchronous behavior, logs, exceptions, and relevant configuration.
4. Determine the underlying cause rather than patching only the visible symptom.
5. Check whether multiple findings share one root cause and whether one fix can safely address them.
6. Identify callers, contracts, data formats, and dependent behavior that may be affected.

Do not modify code merely to remove a warning, silence an error, or make a test pass. A test expectation can be wrong; verify it against documented or established behavior.

## 3. Apply targeted fixes

For each verified bug:

- Make the smallest reasonable change that corrects the root cause.
- Preserve architecture, public interfaces, API contracts, database compatibility, and intended business behavior unless changing them is necessary to fix the defect.
- Address relevant validation, null/undefined handling, error propagation, async control, authorization, consistency, and edge cases.
- Follow project conventions and reuse existing abstractions. Avoid unnecessary dependencies and duplicate logic.
- Consider downstream effects on callers, shared modules, persistence, cached state, and user workflows.
- Add or update focused regression tests when appropriate, without weakening or deleting existing tests.

Before a potentially high-blast-radius change, explain the impact and verify compatibility. If a fix requires a product decision, destructive operation, production access, or a breaking public contract, stop before that action and ask for authorization or the missing decision.

## 4. Handle finding categories correctly

- **Confirmed bug:** Fix when the root cause is understood and a safe change is available. Verify the original failure.
- **High-confidence bug:** Investigate until the cause is established. Fix only when evidence supports the change; otherwise document uncertainty.
- **Potential bug:** Gather evidence first. Do not make speculative changes.
- **Environment/infrastructure failure:** Do not mislabel it as an application bug. Remediate only if within scope, safe, and supported by evidence; otherwise document the blocker.
- **Duplicate/already resolved/not a bug:** Verify current state and record why no code change is needed.

## 5. Verify each fix

After each meaningful change:

- Run the narrowest relevant test or repeat the original reproduction procedure.
- Confirm the incorrect behavior is corrected and expected behavior is preserved.
- Run related unit/integration checks where practical.
- Check relevant type errors, lint errors, compilation/build issues, and runtime exceptions introduced by the change.
- Inspect the diff for accidental or unrelated modifications.
- Map every changed file to the defect it addresses and the verification that covers it.

If a fix fails verification, investigate further. If verification is blocked, label it unverified; do not claim completion. Targeted checks belong in this phase. The broad regression campaign remains the next phase.

## 6. Safety and scope

- Modify code only for verified bugs and directly necessary tests.
- Do not redesign features, refactor unrelated modules, or expand scope.
- Do not silently revert user changes or overwrite unrelated modifications.
- Do not delete data, reset databases, or perform destructive operations against production services.
- Use isolated environments and test data wherever possible.
- Do not expose secrets or credentials.
- Do not install unnecessary dependencies.
- Do not suppress meaningful errors, hardcode test-specific behavior, weaken validation, or alter assertions to produce green results.
- Work through actionable findings in agreed scope, ordered by risk. If the remaining set is too large or a change has material unknown impact, report progress and constraints rather than making unsafe bulk changes.

## 7. Required fix report

### A. Fix summary

Count unique findings reviewed, fixed and verified, changed but not fully verified, investigated but not fixed, already resolved/not bugs, and blocked. Avoid double-counting duplicates.

### B. Changes made

For each fix:

`finding ID → severity/confidence → path/location → evidence → root cause → change made → targeted verification → result`

Clearly separate verified fixes from changes awaiting verification.

### C. Outstanding issues

List remaining confirmed, high-confidence, and potential issues separately. Include why unresolved, risk, missing evidence/dependency, and next action.

### D. Checks executed

List exact commands/procedures and actual results. Distinguish targeted verification from the full regression suite.

### E. Changed files and impact assessment

Summarize files changed, rationale, API/schema/dependency/configuration/compatibility implications, and likely blast radius. Confirm the final diff was inspected.

### F. Handoff to Regression Testing

For every fix, provide:

- Stable finding/fix ID and expected corrected behavior.
- Original reproduction scenario and the test that now covers it.
- Changed files and dependent modules/workflows.
- Related tests to rerun and edge cases to broaden.
- Known baseline failures and outstanding blockers.

### G. Fix phase status

Choose one:

- `COMPLETED`: All actionable verified findings in scope have been fixed and targeted verification passed.
- `PARTIALLY COMPLETED`: Some findings were fixed, but actionable defects remain or some fixes are unverified.
- `BLOCKED`: Essential constraints prevent meaningful progress.
- `NO FIX REQUIRED`: Investigation established that no actionable defects remain in the assessed scope.

Explain the evidence. Targeted success is not proof that the whole application is healthy.

**Stop here.** Do not begin broad regression testing unless the user explicitly requested full-pipeline mode.
