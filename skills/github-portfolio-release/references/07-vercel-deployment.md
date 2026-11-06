# Phase 7 — Vercel deployment and live verification

**Mode:** production-impacting external action. Linking a production project, changing production environment variables, deploying to production, assigning domains, or changing DNS requires explicit approval immediately before execution.

## Objective

Deploy the confirmed GitHub project to Vercel when compatible, then verify the actual live application rather than relying only on deployment status.

## A. Confirm suitability and access

1. Verify the project framework/runtime is supported by the chosen Vercel configuration. Do not assume every backend, Rust service, worker, database, or long-running process can run directly on Vercel.
2. Inspect the build command, output settings, root directory, Node/runtime version, monorepo root, install command, and deployment configuration.
3. Check whether the project is already linked to a Vercel team/project. Do not link it to a different account/team or create a duplicate project without approval.
4. Check available `vercel` CLI/browser/MCP capability and authentication status without exposing credentials. If unavailable, provide exact dashboard steps and mark the action `BLOCKED`; do not claim deployment.
5. Confirm target GitHub repository/branch and deployment environment (preview vs production).

## B. Prepare environment configuration

1. Identify variable names referenced by source/configuration and group them by required/optional and server/client exposure.
2. Distinguish server-only secrets from variables that are intentionally public (for example, framework-prefixed public variables). Never assume a variable is safe to expose based only on its name.
3. Compare required variable names with the target environment's configured names/status if access allows. Never print values.
4. Do not copy local `.env` values into Vercel automatically. User must approve adding or changing production values through the supported secure interface.
5. Do not place secrets in build arguments, public variables, README, logs, or client bundles.

## C. Approval checkpoint

Before linking or deploying, show:

- GitHub repo and branch.
- Vercel team/project target (or proposed new project).
- Framework/root/build/output settings to be applied.
- Environment variable names that appear required and whether they are present/missing (never their values).
- Deployment target: Preview or Production.
- Domain/alias changes, if any.

Wait for explicit approval for the specific external action. Do not interpret “make it deployable” as permission to deploy to production.

## D. Deploy and inspect

After approval and when tools permit:

1. Link/import the confirmed repository/project.
2. Configure only required build settings and approved environment variable names/values through secure tooling.
3. Deploy to the approved target.
4. Capture the deployment URL, deployment ID/status, deployed commit, target environment, and timestamp when available.
5. Open the actual deployed URL in a browser if available. Check HTTP response, primary routes, navigation, image/font/static assets, console/runtime errors, authentication/data connections where feasible, and responsive behavior.
6. Exercise important user flows where safe test accounts/data exist. Do not use destructive actions against production data.
7. Compare the deployed commit to the intended GitHub commit. A successful Vercel “Ready” state alone is insufficient evidence of application health.

## E. Diagnose failures safely

Classify failures as build/configuration, missing environment, framework/runtime incompatibility, network/database integration, application defect, or unavailable verification. Fix only narrow deployment configuration issues within scope. Route application logic defects to `software-quality-pipeline`. Do not change production secrets, database schema, DNS, or data as an exploratory fix.

## Required report

Report target project/team, GitHub repo/branch/commit, build settings, environment variable names/status only, deployment URL/status, live routes and checks actually performed, console/runtime findings, and limitations. Use `PASS`, `FAIL`, `BLOCKED`, or `NOT RUN` for each important verification.
