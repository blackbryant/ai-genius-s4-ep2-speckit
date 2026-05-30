# Implementation Plan: Web Frontend Deployment via GitHub Actions

**Branch**: `003-deploy-web` | **Date**: 2026-05-30 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from `/specs/003-deploy-web/spec.md`

## Summary

Create and finalize GitHub Actions workflow `.github/workflows/002-deploy-web.yml` to deploy the React + Vite frontend in `src/ai-genius-web` to Azure Static Web Apps. The workflow must trigger on `push` to `main` and `workflow_dispatch`, mirror environment and concurrency behavior from `001-deploy-infra.yml`, run `npm ci` and `npm run build`, authenticate with `azure/login@v1` using `AZURE_CREDENTIALS`, and deploy via `Azure/static-web-apps-deploy@v1` using `AZURE_STATIC_WEB_APPS_API_TOKEN`.

## Technical Context

**Language/Version**: YAML (GitHub Actions), JavaScript/JSX (React + Vite), Node.js 20  
**Primary Dependencies**: `actions/checkout@v4`, `actions/setup-node@v4`, `azure/login@v1`, `Azure/static-web-apps-deploy@v1`  
**Storage**: N/A  
**Testing**: Workflow-level verification through successful `npm run build` and deployment run status in GitHub Actions  
**Target Platform**: GitHub Actions `ubuntu-latest`, Azure Static Web Apps  
**Project Type**: Web application CI/CD workflow  
**Performance Goals**: Typical run completion under 5 minutes for install, build, and deploy path  
**Constraints**: Must follow infra workflow environment/concurrency conventions; use mandated secrets and action versions  
**Scale/Scope**: Single workflow file and related docs for one frontend app (`src/ai-genius-web`)

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

| Principle | Pre-Phase-0 | Post-Phase-1 | Notes |
|-----------|-------------|--------------|-------|
| Security-First | PASS | PASS | Secrets stay in GitHub Secrets; no credentials in repo files |
| Cloud-Native | PASS | PASS | Deploy target is Azure Static Web Apps; infra remains IaC-managed |
| CI/CD-Driven | PASS | PASS | Push to `main` + manual dispatch satisfy automation requirements |
| Spec-Gated | PASS | PASS | Spec and planning artifacts are present under `specs/003-deploy-web/` |
| Simplicity | PASS | PASS | Uses standard GitHub and Azure actions without custom frameworks |
| Tested | PASS | PASS | Build step gates deployment; failed build blocks deployment |

No gate violations found.

## Project Structure

### Documentation (this feature)

```text
specs/003-deploy-web/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
│   └── workflow-interface.md
└── tasks.md                    # Phase 2 output (/speckit.tasks)
```

### Source Code (repository root)

```text
.github/
└── workflows/
    └── 002-deploy-web.yml      # Frontend deploy workflow (target of this plan)

src/
└── ai-genius-web/
    ├── package.json
    ├── vite.config.js
    └── src/
        ├── main.jsx
        └── App.jsx
```

**Structure Decision**: Keep scope to one workflow file plus documentation artifacts. No new runtime application directories are introduced.

## Complexity Tracking

No constitution violations or complexity exceptions require justification.
