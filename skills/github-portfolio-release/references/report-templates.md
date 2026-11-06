# Report templates

Use the smallest template that fully communicates the result. Replace every placeholder with evidence; remove fields that are genuinely not applicable. Never leave a placeholder that could be mistaken for a verified result.

## Phase report

```markdown
# [Phase] — [Project]

**Status:** READY / READY WITH RISKS / NEEDS WORK / BLOCKED / NOT RUN  
**Scope:** [root, branch, environment, requested phase]  
**Checks/actions performed:** [actual commands and inspections]

## Findings
| Severity | Confidence | Finding | Evidence / impact |
|---|---|---|---|

## Changes
- Created/modified/moved/deleted: [paths and reason]
- None, if no files changed.

## Risks and limitations
- [confirmed issue, unknown, or unavailable tool]

## Handoff
[recommended next phase or why execution stops]
```

## Phase 1 audit addendum

```markdown
## Project snapshot
- Stack/framework:
- Runtime/package manager:
- Build/test scripts:
- Git branch/remotes:
- Working tree: clean / modified / staged / untracked
- Existing deploy target:

## Repository hygiene
- Secret exposure (working tree):
- Secret exposure (history):
- Generated/local files:
- Large/private assets:
- Missing docs/config:

## Release blockers
- [finding + evidence]
```

## Phase 4 validation matrix

```markdown
| Check | Result | Command/route | Evidence or reason skipped |
|---|---|---|---|
| Install/lockfile | PASS/FAIL/BLOCKED/NOT RUN | | |
| Lint | PASS/FAIL/BLOCKED/NOT RUN | | |
| Typecheck | PASS/FAIL/BLOCKED/NOT RUN | | |
| Tests | PASS/FAIL/BLOCKED/NOT RUN | | |
| Production build | PASS/FAIL/BLOCKED/NOT RUN | | |
| Runtime smoke test | PASS/FAIL/BLOCKED/NOT RUN | | |
| Browser/responsive | PASS/FAIL/BLOCKED/NOT RUN | | |
```

## Phase 5 publish approval preview

```markdown
Before publishing, please approve this exact action:
- GitHub owner/repository:
- Visibility: public/private
- Branch:
- Remote: create new / use existing [URL]
- Commit to publish: [hash/message or pending commit]
- Files included: [staged path list]
- Files explicitly excluded:
- Known secret/history concerns:

No remote repository, branch, or history will be overwritten without separate approval.
```

## Phase 7 deployment approval preview

```markdown
Before deployment, please approve:
- Repository/branch/commit:
- Vercel team/project:
- Environment: Preview / Production
- Framework/root/build/output settings:
- Required environment variable names and status (values omitted):
- Domain/alias changes:
- Expected external action:
```

## Phase 10 scorecard

```markdown
| Dimension | Score (1–5) | Evidence | Confidence |
|---|---:|---|---|
| Product clarity/usefulness | | | HIGH/MEDIUM/LOW |
| UI/UX/accessibility | | | HIGH/MEDIUM/LOW |
| Frontend/application engineering | | | HIGH/MEDIUM/LOW |
| Performance/reliability | | | HIGH/MEDIUM/LOW |
| Repository/developer experience | | | HIGH/MEDIUM/LOW |
| Deployment/production readiness | | | HIGH/MEDIUM/LOW |
| Portfolio presentation | | | HIGH/MEDIUM/LOW |
```
