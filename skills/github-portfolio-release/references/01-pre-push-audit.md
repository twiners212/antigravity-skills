# Phase 1 — Pre-push project audit

**Mode:** read-only. Do not edit, delete, move, format, stage, commit, or install anything in this phase.

## Objective

Establish an evidence-based snapshot of the project before cleanup or publishing. Discover the stack, project health, repository contents, secret exposure risks, and release blockers.

## Procedure

### A. Establish context

1. Confirm repository root and inspect top-level structure. Avoid dumping huge dependency/generated folders.
2. Read project instructions and docs, including `AGENTS.md`, `CLAUDE.md`, contribution notes, architecture docs, and prior release/quality reports.
3. Inspect manifests and lockfiles to identify framework, language, package manager, runtime versions, scripts, workspace/monorepo setup, and build system.
4. Inspect deployment configuration such as `vercel.json`, framework config, Docker files, CI workflows, hosting manifests, and environment-variable references.
5. Inspect Git state: branch, remotes, status, staged and unstaged diff summaries, recent commits, tracked files, ignored files, and untracked files. Preserve all existing work.
6. Determine whether the repository already exists remotely and whether deployment configuration points to an existing project. Do not change either.

### B. Inspect repository hygiene

Look for:

- Tracked `.env` or other secret-bearing files; private keys, certificates, tokens, credentials, database dumps, personal data, local databases, and machine-specific configuration.
- Build output, caches, dependency directories, logs, editor/OS noise, temporary files, test artifacts, coverage reports, and large assets.
- Missing or overly broad ignore rules, accidentally ignored source files, duplicate assets, dead-looking files, and inconsistent naming.
- Missing project essentials: README, license decision, `.env.example`, contribution notes, architecture overview, screenshots/demo link, and deployment instructions.
- Unclear distinction between source files, generated files, local data, and public assets.

### C. Assess secret exposure carefully

1. Check tracked paths and patterns that may contain credentials.
2. Use an available secret scanner (for example, an already installed `gitleaks` or equivalent) if appropriate. Do not install a new scanner without authorization.
3. Check Git history for previously committed sensitive files or likely secret patterns where feasible. Avoid commands that print raw secret values. Capture redacted file/path and commit references only.
4. Do not assume `.gitignore` protects files already tracked or committed.
5. If a real secret may have been exposed, mark it as urgent and recommend revoke/rotate first. Removing it from the current tree does not make the old value safe.

### D. Baseline checks

Run only checks that are already available and reasonably non-destructive. Use existing project scripts and the detected package manager. Do not run a dependency install in this phase. A quick metadata inspection is preferable to an expensive full test suite unless the user asked for it.

Potential checks include manifest/lockfile consistency, configured scripts, existing test/build status from recent trustworthy reports, repository size, unusually large files, and obvious deployment blockers.

## Severity and confidence

- **CRITICAL:** likely public secret exposure, private user data, destructive deployment configuration, or another immediate release-stopping risk.
- **HIGH:** a concrete blocker such as missing required environment variables documentation, broken build configuration, or clearly committed local/private files.
- **MEDIUM:** meaningful maintainability or presentation concern that does not necessarily block publishing.
- **LOW:** minor polish or optional improvement.

Label each finding `CONFIRMED`, `HIGH-CONFIDENCE`, or `POTENTIAL`. A suspicion must not be reported as a confirmed leak or defect.

## Required report

Use the Phase 1 template in `report-templates.md`. Include project/stack/package manager, branch and Git status, modified/untracked paths summary, secret scan scope and limitations, repository hygiene findings, available validation evidence, release blockers, and a prioritized recommendation for phases 2–4.

**Stop after reporting.** No cleanup, code fixes, Git staging, repository creation, or deployment in this phase.
