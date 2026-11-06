# Phase 10 — Final professional review

**Mode:** assessment only unless the user separately requests implementation.

## Objective

Assess the project as three audiences: a recruiter evaluating portfolio quality, a client evaluating usefulness/trust, and a developer evaluating maintainability and engineering judgment.

## Review dimensions and scoring rubric

Score each dimension from 1 to 5. Give a short evidence-based reason and confidence level. Do not calculate a misleading single “industry standard” score; an average may be included only as a directional summary.

| Score | Meaning |
|---|---|
| 1 — Weak | Major gaps make the project difficult to understand, use, run, or trust. |
| 2 — Early | Some fundamentals exist, but important gaps are visible. |
| 3 — Competent | Solid baseline with understandable limitations and a few meaningful gaps. |
| 4 — Strong | Cohesive, well-executed, and credible with only targeted improvements needed. |
| 5 — Excellent | Consistently polished, robust, and well-evidenced for the project's intended scope. |

Assess these dimensions:

1. **Product clarity and usefulness:** target user, problem, use case, and real feature completeness.
2. **UI/UX and accessibility:** hierarchy, consistency, interactions, responsiveness, keyboard/focus, and clarity of states.
3. **Frontend/application engineering:** architecture fit, state/data flow, error handling, maintainability, and implementation quality visible from the inspected scope.
4. **Performance and reliability:** observed loading/runtime behavior, asset efficiency, build stability, and resilience. Do not claim production reliability from a local build alone.
5. **Repository and developer experience:** structure, README, setup, environment documentation, tests, Git hygiene, and ease of contribution.
6. **Deployment and production readiness:** verified deployment, runtime integration, environment configuration, observability/operational concerns, and known limitations.
7. **Portfolio presentation:** visual first impression, demo accessibility, genuine screenshots, project narrative, and differentiation.

## Review lenses

### Recruiter

Can they quickly understand the problem, the candidate's contribution, the complexity, the technologies used, and what is genuinely implemented? Is there a working demo or a clear explanation when demo access is restricted?

### Client/product owner

Does the project solve a coherent problem? Are primary workflows obvious? Are limitations and data/security boundaries understandable? Would the client know what is ready versus prototype-only?

### Developer/technical reviewer

Can another developer install and run it? Are architecture and trade-offs explained? Are tests meaningful? Are errors, auth, data integrity, configuration, and deployment assumptions addressed at a level appropriate to the project?

## Required final report

1. **Overall status:** `READY`, `READY WITH RISKS`, `NEEDS WORK`, or `BLOCKED` for public showcase. Explain what “ready” means for this specific project.
2. **Scorecard:** each dimension, score, evidence, and confidence (`HIGH`, `MEDIUM`, `LOW`).
3. **Strengths:** specific, demonstrated strengths only.
4. **Gaps:** distinguish beginner/intermediate-level gaps only when supported by evidence; do not infer the user's skill level from one project.
5. **Public showcase risks:** secrets/private data, broken demo, misleading README, inaccessible setup, unfinished routes, or unverified claims.
6. **Prioritized roadmap:** P0 blockers, P1 high-impact improvements, P2 polish. For each, include rationale and expected impact.
7. **Verification limits:** what could not be inspected and why.

A polished landing page does not compensate for a broken product, and sophisticated architecture does not compensate for an unclear demo. Judge the whole experience in proportion to the project's stated scope.
