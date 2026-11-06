# Phase 2 — Project cleanup and repository hygiene

**Mode:** scoped local changes only after the user has requested cleanup or authorized the full pipeline. If a proposed change risks deleting user work, ask first.

## Objective

Make the repository safer and cleaner for version control without changing application behavior or removing legitimate project content.

## Procedure

### A. Build a cleanup plan

Use Phase 1 findings. For every proposed change, identify the path, why it is unnecessary or unsafe to publish, whether it is tracked, and whether it can be regenerated. Separate:

- Safe ignore candidates: machine-specific editor files, OS noise, logs, temporary output, and framework caches.
- Generated artifacts: build output, test/coverage output, generated bundles, and caches. Verify they are not source inputs or intentionally committed assets.
- Sensitive/private files: `.env*` containing real values, credentials, local databases, backups, and personal data.
- Potentially removable content: duplicates, abandoned files, old screenshots, unused assets, or obsolete docs. These require stronger evidence and explicit approval before deletion.

### B. Update ignore rules conservatively

1. Read the existing `.gitignore` and the stack's official/default ignore conventions where needed.
2. Add only patterns relevant to the detected stack and project tools. Avoid blanket patterns that can hide source files or required assets.
3. Preserve `.env.example` and other intentionally shareable templates. Typical intent is to ignore local environment files while explicitly allowing a sanitized example, but verify the repository's actual conventions.
4. Check the effect of every new rule against important paths. Use `git check-ignore` or equivalent.
5. Remember that ignore rules do not untrack files already in Git. Do not run `git rm --cached` or rewrite history without explicit approval.

### C. Handle environment documentation

1. Find environment-variable references in source/configuration without displaying actual values.
2. Create or update `.env.example` only when useful and supported by evidence.
3. Include variable names, safe placeholder values, short comments, and whether a variable is required/optional when known.
4. Never copy values from `.env`, shell history, CI, GitHub, or Vercel. Never invent required variables.
5. Do not overwrite a maintained example file without reviewing its current contents.

### D. Remove generated files safely

- Prefer adding correct ignore rules over deleting files.
- Before deleting, verify the path, whether it is tracked, whether a process is using it, whether it can be regenerated, and whether it contains user data.
- Only remove clearly disposable generated artifacts within the authorized scope. Do not delete package lockfiles, migrations, source maps required by deployment, fixtures, test snapshots, uploaded user content, or build inputs by assumption.
- Never run `git clean -fdx`, broad recursive deletion, or shell wildcards that could remove unrelated files.
- If a tracked file should stop being versioned, explain the index/history implications and obtain approval.

### E. Validate cleanup

Inspect `git status`, diff, ignore behavior, and staged state. Confirm required source/config/assets remain visible to Git. Ensure no secret values entered `.env.example` or documentation. Run quick non-destructive checks appropriate to the changes.

## Required report

List each created/modified/deleted path with its reason; show important ignore-rule decisions; report environment-variable names only; confirm whether sensitive tracked/history items remain; describe checks performed; identify anything intentionally left untouched and why.

**Stop after cleanup.** Do not organize source folders or publish to GitHub in this phase unless separately requested.
