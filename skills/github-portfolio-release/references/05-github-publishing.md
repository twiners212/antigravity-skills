# Phase 5 — Git setup and GitHub publishing

**Mode:** external action. Repository creation, visibility selection, commit of ambiguous user changes, and push require explicit approval immediately before execution.

## Objective

Publish only the intended, reviewed project state to the intended GitHub repository and branch.

## Procedure

### A. Preflight

1. Inspect `git status`, current branch, staged/unstaged/untracked files, remotes, recent commits, and repository root. Never assume the working tree is clean.
2. Confirm whether this is a new repository or an existing remote. If a remote exists, inspect its URL/owner/repository and current branch relationship before changing anything.
3. Check `gh --version` and `gh auth status` when GitHub CLI is available. Do not ask the user to paste tokens or secrets into chat or terminal logs.
4. Identify the intended repository name, owner/account, visibility (`public` or `private`), default branch, and project description. Do not infer that a portfolio project must be public.
5. Check for secret exposure in the current tree and Git history. If a secret was committed, stop publication and recommend revoke/rotate. Do not treat `.gitignore` as retroactive protection.

### B. Prepare the publish set

1. Review `.gitignore`, environment templates, generated files, local data, private assets, and documentation.
2. Stage only explicitly selected paths. Prefer path-specific `git add <path>` over `git add .` or `git add -A`.
3. Inspect `git diff --cached --stat` and the complete staged diff. Review sensitive-looking file names and content carefully without displaying secret values.
4. Ensure pre-existing staged changes are not accidentally included. If staged work belongs to the user or its intent is unclear, preserve it and ask how to proceed.
5. Do not amend, squash, reset, cherry-pick, or rewrite existing commits unless separately requested and approved.

### C. Branch and commit

- Preserve the existing branch by default. Use `main` only when the repo is new or the user has requested it and the change is safe.
- Do not rename a branch or overwrite an established branch convention without approval.
- Prepare a specific conventional commit message describing the actual changes, not generic claims such as “final production ready”.
- Before committing, show the selected file list and staged summary. Obtain approval if the commit includes any pre-existing user work or if the requested authorization did not clearly include committing.

### D. Create or connect the remote

Before execution, present a concise approval summary:

- GitHub owner/account and repository name.
- Public/private visibility.
- Branch to publish.
- Whether a new repository will be created or an existing remote used.
- Exact local commit(s) and file scope to be pushed.

Only after explicit approval:

1. Create the repo with `gh` if needed, or use the confirmed existing repository.
2. Add/update remote only as approved. Do not overwrite an existing remote silently.
3. Push the intended branch. Never force-push as a convenience. If histories diverge, stop, diagnose, and propose safe options.
4. Verify push result using Git and GitHub CLI/API evidence if available.

### E. Verify the published result

Confirm remote URL, branch, latest commit hash/message, repository visibility, default branch, README presence/rendering, and top-level file tree where supported. Ensure the pushed commit matches the reviewed local commit.

## Stop conditions

Stop and report `BLOCKED` if credentials are unavailable, repo ownership is ambiguous, secret exposure is unresolved, histories diverge unexpectedly, remote content would be overwritten, or the publish set contains unexplained user changes.

## Required report

State repository URL, owner/name, visibility, branch, remote, commit hash/message, publish status, verified remote state, and any files intentionally excluded. Never include auth tokens or secret values.
