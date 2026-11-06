# Software Quality Pipeline — Antigravity Skill

A modular Antigravity skill for an evidence-based five-phase software quality workflow:

1. **Audit** — find and classify defects without changing code.
2. **Testing** — verify actual behavior and identify coverage gaps.
3. **Debugging & Bug Fixing** — establish root causes and apply targeted fixes.
4. **Regression Testing** — verify fixes and detect unintended breakage.
5. **Final Verification** — independently assess project health and readiness.

The root `SKILL.md` is the orchestrator. Detailed phase instructions are stored under `references/` and should be loaded only when needed (progressive disclosure).

## Contents

```text
software-quality-pipeline/
├── SKILL.md
├── README.md
└── references/
    ├── audit.md
    ├── testing.md
    ├── debugging.md
    ├── regression-testing.md
    └── final-verification.md
```

## Install globally in Antigravity IDE

Copy the entire `software-quality-pipeline` folder to:

```text
~/.gemini/config/skills/software-quality-pipeline/
```

On Windows, `~` usually resolves to your user profile directory, for example:

```text
C:\Users\<your-user>\.gemini\config\skills\software-quality-pipeline\
```

Keep the folder name and internal structure intact. Do not copy only the `SKILL.md`; the phase reference files are required by the orchestrator.

## Workspace-only alternative

To make the skill available only inside one project, copy the same folder to:

```text
<project-root>/.agents/skills/software-quality-pipeline/
```

## Usage

Invoke a single phase when needed:

```text
/software-quality-pipeline audit
/software-quality-pipeline testing
/software-quality-pipeline debugging
/software-quality-pipeline regression
/software-quality-pipeline final-verification
```

To explicitly authorize the full sequence:

```text
/software-quality-pipeline full-pipeline
```

The skill does not automatically advance to the next phase when only one phase was requested. Each phase has its own permission boundary and stopping point.

## Operating model

- Audit and Testing are investigation/verification phases; they do not fix production code.
- Debugging is the only phase that authorizes targeted production-code fixes, and only for verified in-scope defects.
- Regression Testing and Final Verification are verification-only phases.
- Test-only changes are narrowly permitted in Testing and Regression Testing when necessary and must be reported.
- Reports are delivered in the conversation by default. Persistent report files are created only when requested or when the project already has an established reporting convention.
- The skill preserves user changes, avoids destructive production operations, redacts secrets, records exact checks, and makes uncertainty explicit.

## Relationship to Infrastructure Reliability Review

Use the separate `infrastructure-reliability-review` skill for infrastructure/operational reliability, capacity, CI/CD resilience, observability, bottleneck analysis, and proactive incident-risk assessment. This skill focuses on application correctness and the software quality lifecycle. Run both when a task genuinely spans both domains, keeping their findings distinct.
