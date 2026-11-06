# Phase 3 — Project organization and documentation

**Mode:** scoped local changes. Favor documentation improvements over structural refactors.

## Objective

Make the project understandable to another developer without forcing an artificial folder structure or breaking imports, routing, migrations, or build assumptions.

## Procedure

### A. Understand the existing architecture

Inspect the framework's conventions and current source layout before recommending moves. Identify entry points, routing, components, services, data access, state, tests, assets, migrations, scripts, and deployment config. In a monorepo, document package boundaries instead of flattening them.

### B. Decide whether structural changes are justified

- Keep an existing structure when it is clear and idiomatic.
- Avoid moving files only to match a generic template such as `app/`, `src/`, `components/`, `lib/`, `hooks/`, `public/`, `docs/`, and `styles/`.
- Prefer small, low-risk improvements: remove confusing duplication only with evidence, clarify names when references can be updated safely, and group new documentation consistently.
- Before moving source files or changing aliases/imports, provide the move plan and obtain approval. If approved, update every reference and run relevant checks.
- Do not delete “unused” files based only on a text search. Check dynamic imports, framework discovery, route conventions, generated references, and build-time usage.

### C. Create or improve project documentation

Create/update these files when they add real value and do not conflict with existing docs:

**`README.md`**
- Project name and concise, accurate purpose.
- Problem/use case and real implemented features.
- Technology stack, grouped by role rather than a noisy badge wall.
- Prerequisites and installation/run/build/test commands confirmed from project manifests.
- Environment setup referencing `.env.example`; never include values.
- Concise architecture/folder overview and real demo/deployment link if one exists.
- Screenshots only if genuine project screenshots are available and user approves their use.

**`architecture.md`**
- System overview and major boundaries.
- Request/data flow and important integrations.
- Persistence/authentication/deployment shape where relevant.
- Key design decisions and known constraints.
- Link to diagrams only if they are accurate and maintained.

**`progress_report.md`**
- Current implemented state, known incomplete work, recent meaningful changes, known issues, and next steps.
- Distinguish implemented, in progress, planned, and unverified features.
- Avoid stale statements and fabricated completion percentages.

Use existing naming or documentation conventions when present; do not create duplicate files such as both `Architecture.md` and `architecture.md` without a clear reason.

### D. Preserve accuracy and ownership

- Do not claim production readiness, security, test coverage, performance, or user counts without evidence.
- Do not invent screenshots, demo URLs, feature descriptions, or contributors.
- Do not add an open-source license automatically. Explain license options and request the owner's choice; a license has legal implications.
- Do not add external images, badges, analytics, or tracking without a clear benefit and authorization.

### E. Validate documentation and moves

Check Markdown links and commands against actual files/scripts. Confirm all imports and route conventions after any approved move. Review the diff for unintended formatting or large unrelated changes.

## Required report

Summarize the organization decision (kept as-is or changed and why), documentation created/updated, source moves (if approved), validation results, and remaining documentation gaps.
