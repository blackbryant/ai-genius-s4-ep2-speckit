# Phase 0 Research — Web Frontend Deployment

All clarifications from `spec.md` are resolved for this feature. This document records pragmatic standard-practice choices for deploying the React + Vite frontend with GitHub Actions.

## Decisions

### D1: Build runtime and toolchain
- **Decision**: Use `actions/setup-node@v4` with Node.js 20 and npm cache keyed by `src/ai-genius-web/package-lock.json`.
- **Rationale**: Matches repository standards and keeps builds fast and reproducible.
- **Alternatives considered**:
  - Node.js 18: older LTS and inconsistent with existing standards.
  - pnpm/yarn: adds package manager complexity not needed for current project.

### D2: Trigger and execution model
- **Decision**: Trigger on `push` to `main` and `workflow_dispatch`; keep environment selection and concurrency behavior aligned with `001-deploy-infra.yml`.
- **Rationale**: Delivers automatic deployments while preserving manual control and consistency across workflows.
- **Alternatives considered**:
  - Push-only trigger: lacks manual redeploy support.
  - Manual-only trigger: does not satisfy continuous deployment expectation.

### D3: Build then deploy artifact
- **Decision**: Run `npm ci` and `npm run build`, then deploy `dist/` through `Azure/static-web-apps-deploy@v1` with `skip_app_build: true`.
- **Rationale**: Clear separation between build failure and deploy failure; deterministic artifact.
- **Alternatives considered**:
  - Let SWA action build via Oryx: less control over build-time variables and build behavior.

### D4: Azure authentication and deployment credentials
- **Decision**: Use `azure/login@v1` with `${{ secrets.AZURE_CREDENTIALS }}` and deploy with `${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}`.
- **Rationale**: Explicitly required by feature request and aligned with current repo secret management.
- **Alternatives considered**:
  - OIDC login: viable but out of scope for this specific requirement set.

### D5: Environment alignment with infra workflow
- **Decision**: Resolve deployment environment with `${{ github.event.inputs.environment || 'dev' }}` and expose runtime env fields for summary consistency.
- **Rationale**: Keeps behavior consistent with `001-deploy-infra.yml` and makes environment context visible in run output.
- **Alternatives considered**:
  - Hardcoding environment names in steps: brittle and not reusable across dev/qa/prod.

### D6: Deployment summary output
- **Decision**: Emit summary line containing `APP_NAME`, resolved `ENVIRONMENT`, and `static_web_app_url`.
- **Rationale**: Gives fast operator confirmation without opening action logs step by step.
- **Alternatives considered**:
  - No summary: slower manual verification and weaker observability for operations.

## Needs Clarification Resolution

No `NEEDS CLARIFICATION` items remain in technical context.
