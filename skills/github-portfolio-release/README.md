# GitHub Portfolio Release

A stack-aware Agent Skill for safely preparing software projects for GitHub, deployment, and professional portfolio presentation.

It combines ten phases: pre-push audit, repository cleanup, project organization, final validation, GitHub publishing, repository finalization, Vercel deployment, portfolio optimization, maintenance planning, and a final professional review.

## Design principles

- **Understand first:** inspect the actual project and its conventions before proposing changes.
- **Progressive disclosure:** keep the main `SKILL.md` focused and load detailed phase guidance only when needed.
- **Preserve user work:** do not overwrite, delete, stage, or publish unrelated changes.
- **Protect secrets:** redact values and account for secrets already present in Git history.
- **Explicit release approvals:** repository creation, visibility, push, production configuration, and deployment require approval immediately before execution.
- **Evidence over assumptions:** report only checks actually performed; do not fabricate tests, metrics, screenshots, or deployment health.
- **No forced conventions:** adapt to the framework, lockfile, hosting platform, and existing architecture.

## Contents

```text
github-portfolio-release/
├── SKILL.md
├── README.md
└── references/
    ├── 01-pre-push-audit.md
    ├── 02-project-cleanup.md
    ├── 03-project-organization.md
    ├── 04-final-validation.md
    ├── 05-github-publishing.md
    ├── 06-repository-finalization.md
    ├── 07-vercel-deployment.md
    ├── 08-portfolio-optimization.md
    ├── 09-maintenance-workflow.md
    ├── 10-professional-review.md
    └── report-templates.md
```

## Install in Antigravity IDE

For a global skill available across workspaces, copy the complete `github-portfolio-release` folder to:

```text
%USERPROFILE%\.gemini\config\skills\github-portfolio-release\
```

The final layout should be:

```text
%USERPROFILE%\.gemini\config\skills\github-portfolio-release\SKILL.md
%USERPROFILE%\.gemini\config\skills\github-portfolio-release\README.md
%USERPROFILE%\.gemini\config\skills\github-portfolio-release\references\...
```

For a project-only skill, copy the folder to:

```text
<project-root>\.agents\skills\github-portfolio-release\
```

Keep the `references` directory beside `SKILL.md`; the main file links to those files by relative path. After installation, refresh/reopen the agent session if the skill is not detected, then check Antigravity's skills/customizations view.

## Suggested invocations

```text
/github-portfolio-release
/github-portfolio-release audit
/github-portfolio-release cleanup
/github-portfolio-release validate
/github-portfolio-release github
/github-portfolio-release vercel
/github-portfolio-release review
```

Or use natural language, for example:

> Use `github-portfolio-release` to run phase 1 only. Audit this project before publishing. Do not modify files.

> Use `github-portfolio-release` for the full pipeline, but stop for approval before creating a GitHub repository, pushing, changing production settings, or deploying.

The exact invocation UX can vary by Antigravity version; the skill name and phase request in natural language are sufficient.

## Relationship to other skills

- `software-quality-pipeline` is for systematic application audit, testing, debugging, regression testing, and final code verification.
- `infrastructure-reliability-review` is for preventive architecture and infrastructure reliability analysis.
- `github-portfolio-release` is for repository hygiene, release preparation, GitHub/Vercel delivery, maintenance guidance, and portfolio presentation.

Reuse their findings when current; do not repeat the same checks without a reason.

## Scope and limitations

This skill provides workflow instructions, not credentials or guaranteed tool access. Publishing and deployment require the relevant CLI/MCP/browser capability, authenticated accounts, project access, and explicit user approval. It must report unavailable checks honestly. It is not a substitute for security review, legal review of licensing, or production operations approval.
