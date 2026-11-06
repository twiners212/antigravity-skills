# Phase 8 — Portfolio optimization

**Mode:** review and recommendations by default. Apply UI, content, asset, or deployment changes only when requested and within the approved scope.

## Objective

Improve the project's first impression and clarity for recruiters, clients, and other developers without resorting to generic “AI slop” or cosmetic changes that obscure the actual product.

## A. Review the public experience

Inspect the live deployment (if verified), README, repository tree, screenshots, and project documentation. Evaluate:

- First 10–20 seconds: can a visitor understand the product, intended user, and main value?
- Visual hierarchy: typography, spacing, alignment, information density, and consistent component behavior.
- Design quality: coherent visual language, intentional color, restrained effects, and purposeful motion. Avoid generic gradient backgrounds, excessive glass cards, meaningless stats, and decorative sections that add no information.
- Product clarity: real use cases, realistic data, understandable empty/error/loading states, and visible primary workflow.
- Responsive quality: mobile navigation, content overflow, forms, tables, and touch targets.
- Accessibility basics: semantic landmarks, keyboard support, focus, labels, reduced-motion considerations, and readable contrast.
- Performance: image dimensions/formats, loading behavior, font strategy, bundle/asset size, unnecessary animation, and layout shift where measurable.
- Trust: accurate copy, real screenshots, working links, clear demo limitations, and no exposed private information.

## B. Review repository presentation

Check README scannability, architecture documentation, installation clarity, commit history, directory organization, dependency footprint, and whether a reviewer can run or understand the project without private context.

## C. Prioritize recommendations

Use three groups:

- **P0 — blocker:** breaks the demo, hides the product's purpose, exposes private data, or makes the repository misleading.
- **P1 — high impact:** materially improves first impression, core workflow clarity, mobile usability, or setup reliability.
- **P2 — polish:** minor spacing, copy, asset, or optional animation refinements.

Each recommendation should state the observed evidence, user/reviewer impact, proposed change, and effort/risk estimate (low/medium/high). Avoid broad redesign proposals without a concrete problem.

## D. Avoid fake polish

Do not fabricate testimonials, usage statistics, awards, client logos, screenshots, product metrics, or deployment badges. Do not add badges simply to decorate the README. Do not optimize for animation at the expense of readability, performance, reduced-motion users, or accessibility.

## Required report

Provide an ordered list of high-impact improvements and a brief assessment of current presentation. If scoring is requested, use the rubric in `10-professional-review.md`, cite evidence, and label uninspected areas as unknown rather than guessing.
