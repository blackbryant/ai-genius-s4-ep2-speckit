# Quickstart — Deploy Web Frontend Workflow

## 1. Prerequisites

- Azure Static Web App is already provisioned.
- Repository secret `AZURE_CREDENTIALS` is configured.
- Repository secret `AZURE_STATIC_WEB_APPS_API_TOKEN` is configured.
- Frontend project exists at `src/ai-genius-web`.

### 1.1 Environment variables per GitHub Environment

Create `dev`, `qa`, and `prod` GitHub Environments and define:

- `APP_NAME` (example: `aigenius4`)
- `VITE_API_URL` (example dev: `https://aigenius4-api-dev.azurewebsites.net`)

The workflow resolves environment with `${{ github.event.inputs.environment || 'dev' }}`.

## 2. Local Build Validation

```bash
cd src/ai-genius-web
npm ci
npm run build
```

Expected result: build completes and outputs files to `dist/`.

## 3. Automatic Deployment

Push changes to `main`:

```bash
git push origin main
```

Expected result: workflow `002-deploy-web.yml` starts automatically.

## 4. Manual Deployment

1. Open GitHub Actions.
2. Select workflow `002-deploy-web`.
3. Run workflow via `workflow_dispatch`.
4. Select target environment (`dev` / `qa` / `prod`).

Expected result: workflow builds and deploys frontend artifact to Azure Static Web Apps.

## 5. Verification

- Confirm workflow run status is success.
- Open deployed site URL and verify latest frontend changes are visible.
- Confirm summary shows deployed application name and resolved environment.

## 6. Concurrency Validation

1. Trigger two manual runs on the same branch/ref in quick succession.
2. Confirm concurrency group `${{ github.workflow }}-${{ github.ref }}` is applied.
3. Confirm previous in-progress run is canceled (`cancel-in-progress: true`).
