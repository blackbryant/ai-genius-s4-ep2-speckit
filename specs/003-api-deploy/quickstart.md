# Quickstart: Backend API Deployment

## Prerequisites

- GitHub repository secret `AZURE_CREDENTIALS` (service principal JSON).
- GitHub repository variable `APP_SERVICE_NAME` configured for each environment (`dev`, `qa`, `prod`).
- App Service already provisioned (run `001 Deploy Infra` first).

## Trigger the workflow

1. **Push to main** — auto-deploys to `dev`.
2. **Manual** — Actions → `003 Deploy API to Azure` → Run workflow → choose `dev`/`qa`/`prod`.

## Validate

- Check the workflow run completes successfully.
- Open the App Service URL and confirm the API responds.

## Troubleshooting

| Symptom | Likely cause |
|---------|--------------|
| `dotnet publish` fails | Compile error in `src/ai-genius-api`. |
| `azure/login` fails | `AZURE_CREDENTIALS` missing or invalid. |
| `webapps-deploy` fails with name not found | `APP_SERVICE_NAME` variable unset for the selected environment. |
