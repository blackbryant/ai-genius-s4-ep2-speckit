# Phase 1 Data Model — Web Frontend Deployment

This feature introduces a workflow-level operational model rather than persistent business data.

## Entities

### 1. Deployment Workflow (`.github/workflows/002-deploy-web.yml`)

| Field | Type | Source | Notes |
|---|---|---|---|
| `name` | string | literal | Workflow display name |
| `on.push.branches` | list | literal | Must include `main` |
| `on.workflow_dispatch` | object | literal | Manual trigger configuration |
| `on.workflow_dispatch.inputs.environment` | enum | literal | `dev` / `qa` / `prod` |
| `concurrency.group` | expression | literal | Must match infra workflow pattern |
| `concurrency.cancel-in-progress` | boolean | literal | Prevents conflicting runs |

### 2. Deployment Run

| Field | Type | Source | Notes |
|---|---|---|---|
| `trigger` | enum | github event | `push` or `workflow_dispatch` |
| `targetEnvironment` | string | workflow expression | Aligned to infra environment behavior |
| `buildStatus` | enum | step result | success/failure |
| `deployStatus` | enum | step result | success/failure |
| `completedAt` | datetime | github runtime | Run end timestamp |

### 3. Credentials and Tokens

| Field | Type | Scope | Required |
|---|---|---|---|
| `AZURE_CREDENTIALS` | secret | repository | yes |
| `AZURE_STATIC_WEB_APPS_API_TOKEN` | secret | repository | yes |

### 4. Environment Variables

| Field | Type | Scope | Required |
|---|---|---|---|
| `APP_NAME` | variable | GitHub Environment | yes |
| `VITE_API_URL` | variable | GitHub Environment | yes |
| `ENVIRONMENT` | runtime env | workflow env | yes |

### 5. Frontend Artifact

| Field | Value |
|---|---|
| Source | `src/ai-genius-web` |
| Build command | `npm run build` |
| Output directory | `src/ai-genius-web/dist` |
| Deployment target | Azure Static Web Apps |

### 6. Deployment Output

| Field | Type | Source | Notes |
|---|---|---|---|
| `static_web_app_url` | string | `steps.swa.outputs.static_web_app_url` | Written to GitHub step summary |

## State Transitions

```text
push to main / manual dispatch
  -> checkout
  -> setup node
  -> npm ci
  -> npm run build
  -> azure login
  -> static web app deploy
  -> run success
```

Any failed step transitions directly to workflow failure.
