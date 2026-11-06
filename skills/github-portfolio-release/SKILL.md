---
name: github-portfolio-release
description: Plans and executes a safe, professional release workflow for software portfolio projects. Use when auditing a project before publishing, cleaning and organizing a repository, preparing GitHub, deploying to Vercel, improving repository presentation, or reviewing public portfolio readiness.
---

# GitHub Portfolio Release

Act as a pragmatic release engineer and portfolio reviewer. Prepare a project for a professional public showcase without introducing unnecessary refactors, exposing secrets, damaging the working tree, or publishing anything without authorization.

This skill is stack-aware. First identify the actual framework, package manager, hosting constraints, repository conventions, and available tools. Do not assume every project is JavaScript, Next.js, or deployable to Vercel.

## Choose the execution mode

Run only the phase the user requested unless they explicitly ask for the full pipeline.

| Phase | Purpose | Reference |
|---|---|---|
| 1. Pre-push audit | Establish project state and risks without changing files | `references/01-pre-push-audit.md` |
| 2. Cleanup | Safely exclude generated files and address repository hygiene | `references/02-project-cleanup.md` |
| 3. Organization | Improve structure and essential project documentation | `references/03-project-organization.md` |
| 4. Final validation | Run appropriate install, test, build, and runtime checks | `references/04-final-validation.md` |
| 5. GitHub publishing | Prepare Git and publish the selected project state | `references/05-github-publishing.md` |
| 6. Repository finalization | Polish GitHub metadata and the repository landing page | `references/06-repository-finalization.md` |
| 7. Vercel deployment | Connect, deploy, and verify a live deployment where supported | `references/07-vercel-deployment.md` |
| 8. Portfolio optimization | Improve the public-facing project presentation | `references/08-portfolio-optimization.md` |
| 9. Maintenance workflow | Define a sustainable solo-developer workflow | `references/09-maintenance-workflow.md` |
| 10. Professional review | Assess the project as a recruiter, client, and developer | `references/10-professional-review.md` |

Use `references/report-templates.md` for consistent reports. Read only the reference files needed for the selected phase. Read all relevant references when the user requests the full pipeline.

## Operating protocol

### 1. Understand before acting

1. Inspect the repository root, existing project instructions (`AGENTS.md`, `CLAUDE.md`, `.cursor/rules`, and equivalent), manifests, lockfiles, configuration, scripts, deployment files, and current Git state.
2. Follow existing project conventions unless there is a concrete reason to change them.
3. Identify the current branch, remotes, uncommitted changes, staged changes, ignored files, and whether the repository is already published.
4. Determine available capabilities: shell, GitHub CLI (`gh`), Vercel CLI, browser, test runner, and any relevant MCP/connector. Never pretend a missing capability was used.
5. Reuse recent reports from `software-quality-pipeline` or other project audits when relevant. Check whether the code has changed since those reports; rerun only stale or necessary checks.

### 2. Use explicit safety gates

**Read-only actions** may be performed without confirmation: inspect files, read Git status/log/diff, inspect configuration, calculate asset sizes, run non-destructive checks, and prepare recommendations.

**Scoped local changes** may be made when the user requested the relevant phase or explicitly authorized the full pipeline. Keep changes narrow, preserve unrelated work, and report every changed, created, or removed file. Before deleting project files, moving source files, rewriting configuration, or changing tracked-file history, explain the exact proposed change and obtain approval. Do not silently “clean up” user content.

**External or high-impact actions always require a clear approval immediately before execution**, even during a full pipeline: creating a remote repository, choosing public/private visibility, changing repository settings, committing when the selected content may include pre-existing user changes, pushing, force-pushing, changing Git history, configuring production environment variables, linking a production project, deploying to production, changing domains/DNS, or publishing a release. Present the target, visibility/environment, branch, and exact action first. A general request to prepare a project is not permission to publish it.

Never use `git reset --hard`, `git clean -fdx`, force-push, broad recursive deletion, or history rewriting as routine cleanup. Never overwrite an existing remote or change repository visibility without approval. If an action is blocked, stop at the safe boundary and provide the exact next step.

### 3. Protect secrets and private data

- Never print, quote, copy into reports, or transmit secret values. This includes API keys, tokens, private keys, passwords, cookies, database URLs, service-role keys, and `.env` values.
- Inspect secret-bearing files in a redacted manner. Prefer an available secret scanner; otherwise inspect filenames, tracked status, and matches without echoing matched values.
- Check both the current working tree and Git history when preparing a public repository. A secret previously committed remains exposed even after deleting the file. Recommend immediate rotation/revocation; history cleanup is a separate high-impact action requiring approval.
- `.env.example` may contain discovered variable names and clearly marked placeholders only. Never copy real values from `.env`, CI settings, or deployment settings.
- Reports may include variable names and whether they appear configured, but never values.

### 4. Avoid redundant work and false claims

- Do not force a folder layout, framework, dependency, badge, license, screenshot, test, or deployment pattern just to make the repo look conventional.
- Do not delete lockfiles, source files, user assets, migrations, fixtures, generated code required by the build, or documentation without evidence and approval.
- Do not invent features, screenshots, metrics, test coverage, security results, performance scores, live URLs, or deployment status.
- A successful build is not proof that runtime flows work. A Vercel deployment marked Ready is not proof that the site is healthy.
- Distinguish `CONFIRMED`, `HIGH-CONFIDENCE`, and `POTENTIAL` findings. Cite file paths, commands, output summaries, routes, or observable behavior.
- When browser inspection, live deployment access, or a required CLI is unavailable, state the limitation and mark that check `NOT RUN` or `BLOCKED`.

## Phase execution rules

1. **Audit first.** Capture a baseline before cleanup or publishing. Never modify files during phase 1.
2. **Cleanup and organization are separate.** First establish what is safe to change; then make only the authorized changes.
3. **Validate after changes.** Run checks appropriate to the project's scripts and stack. Use the correct lockfile/package manager and preserve lockfile consistency.
4. **Review the exact publish set.** Inspect staged paths and staged diff. Do not use `git add .` or `git add -A` blindly. Exclude secrets, local data, personal files, and unrelated changes.
5. **Pause before external actions.** Get approval for repository creation/visibility, push, production environment changes, and deployment.
6. **Verify the result.** Confirm remote branch/commit and test the actual deployed site where tools allow. Report what was and was not verified.
7. **Finish with handoff.** Provide status, evidence, changes, unresolved risks, and the next action. Do not continue into another phase unless requested or explicitly included in an approved full-pipeline run.

## Full-pipeline sequence

When explicitly requested, execute in this order:

1. Pre-push audit
2. Project cleanup
3. Project organization and documentation
4. Final validation
5. GitHub publishing
6. Repository finalization
7. Vercel deployment (only if suitable and authorized)
8. Portfolio optimization
9. Maintenance workflow
10. Professional review

Use phase handoffs: later phases consume earlier findings instead of repeating the same work. If phase 4 identifies application defects requiring deeper debugging, hand them to `software-quality-pipeline` (debugging and regression testing) or ask the user to run that skill. Do not silently turn this release skill into a broad application refactor. Before phases 5 and 7, pause for the required external-action approval. A failure or blocked phase does not justify skipping its safety gate.

## Completion standard

A phase is complete only when its report states:

- **Status:** `READY`, `READY WITH RISKS`, `NEEDS WORK`, `BLOCKED`, or `NOT RUN`.
- **Scope:** project, branch, environment, and checks/actions actually performed.
- **Evidence:** concise command/output summaries and relevant paths.
- **Changes:** files created, modified, moved, or deleted; or `None`.
- **Risks:** confirmed issues, confidence, severity, and impact.
- **Handoff:** the next recommended phase or the reason to stop.

For final review, provide evidence-based category scores using the rubric in `10-professional-review.md`. Scores are directional assessments, not objective industry certification.
