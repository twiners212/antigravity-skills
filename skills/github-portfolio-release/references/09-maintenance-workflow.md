# Phase 9 — Maintenance workflow

**Mode:** recommendations/documentation. Do not change branch protection, CI, dependency versions, or repository automation without approval.

## Objective

Give a solo developer a lightweight repeatable path from local development to verified release, without importing unnecessary enterprise process.

## A. Recommended solo workflow

A practical default:

1. Keep `main` as the stable branch intended to deploy.
2. For meaningful work, use short-lived branches such as `feat/<name>`, `fix/<name>`, or `chore/<name>`. Tiny changes may be committed directly to `main` when the repository is personal and no collaboration/protection policy requires branches.
3. Run the narrowest relevant check while developing; before merging/pushing to the production branch, run typecheck/lint/tests/build as configured.
4. Review `git diff`, `git status`, and staged paths before committing.
5. Use clear commit messages that describe one logical change.
6. Push and verify CI/preview deployment. Promote to production only after checking the target branch, environment, and live behavior.
7. Keep a short `progress_report.md` or issue list only if it remains current and useful.

Do not impose GitFlow or create `develop`, `release`, and hotfix branches for a solo project unless there is a real release-management need.

## B. Lightweight daily checklist

- Confirm the intended branch and working tree.
- Pull/rebase only when appropriate; avoid overwriting local work.
- Implement one coherent change.
- Run relevant tests and inspect runtime behavior.
- Review diff and remove debug-only output or local data.
- Commit with a specific message.
- Push only to the intended remote/branch after approval where required by the active task.
- Verify preview/production deployment status if configured.

## C. Dependency maintenance

1. Identify the actual package manager and dependency policy.
2. Inspect direct dependencies and known available update tooling. Do not install new audit tools without authorization.
3. Separate security advisories, incompatible major upgrades, ordinary minor/patch updates, and unused-dependency suspicions.
4. Do not remove a dependency solely because a text search found no import: it may be used by configuration, plugins, scripts, dynamic resolution, or framework conventions.
5. Update one logical group at a time, preserve the lockfile, and run affected tests/build after each update.
6. Avoid “update everything” as routine maintenance, especially before a release.

## D. When to refactor or split

Refactor when there is repeated change friction, clear duplicated behavior, boundaries are difficult to test, or a module has multiple unrelated responsibilities. Split a package/service only when independent deployment, ownership, scaling, or dependency boundaries justify the operational cost. Do not split merely because a folder looks large.

## E. Maintenance automation ideas

Recommend CI checks such as lint/typecheck/test/build, dependency update review, secret scanning, and preview deployment only where supported and maintainable. Avoid adding workflows that leak secrets, run untrusted code with privileged tokens, or deploy every branch to production.

## Required report

Provide the suggested branch/commit convention, pre-push checklist, dependency review cadence, CI ideas, and what should remain manual. Clearly separate recommendations from changes actually applied.
