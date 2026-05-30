# Workflow Interface Contract — `002-deploy-web.yml`

This contract defines observable workflow inputs, required secrets, and deploy outputs.

## Triggers

| Trigger | Configuration |
|---|---|
| `push` | `branches: [main]` |
| `workflow_dispatch` | Input `environment` choice: `dev`, `qa`, `prod` (default: `dev`) |

## Concurrency

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

## Required Secrets

| Name | Required | Purpose |
|---|---|---|
| `AZURE_CREDENTIALS` | yes | Used by `azure/login@v1` |
| `AZURE_STATIC_WEB_APPS_API_TOKEN` | yes | Used by `Azure/static-web-apps-deploy@v1` |

## Required Variables

| Name | Scope | Required | Purpose |
|---|---|---|---|
| `APP_NAME` | GitHub Environment variable | yes | Deployment summary output |
| `VITE_API_URL` | GitHub Environment variable | yes | Frontend build-time API endpoint |

## Job Environment Resolution

- Job `environment`: `${{ github.event.inputs.environment || 'dev' }}`
- Workflow `env.ENVIRONMENT`: `${{ github.event.inputs.environment || 'dev' }}`

## Build and Deploy Surface

| Contract Item | Value |
|---|---|
| Build root | `src/ai-genius-web` |
| Dependency install | `npm ci` |
| Build command | `npm run build` |
| Artifact path | `src/ai-genius-web/dist` |
| Deploy action | `Azure/static-web-apps-deploy@v1` |

## Outputs

| Output | Source | Description |
|---|---|---|
| `static_web_app_url` | `steps.swa.outputs.static_web_app_url` | URL of deployed frontend shown in job summary |

## Expected Behavior

- Workflow must run for every push to `main`.
- Workflow must support manual dispatch.
- Workflow environment and concurrency behavior must match infra workflow conventions.
- Any build or deployment failure must fail the run.
- Deployment summary must include app name, resolved environment, and deployed URL.
