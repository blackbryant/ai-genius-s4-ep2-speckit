# Feature Specification: Deploy AI Genius Web Frontend

**Feature Branch**: `003-deploy-web`  
**Created**: 2026-05-30  
**Status**: Draft  
**Input**: User description: "Deploy the AI Genius React frontend web app via GitHub Actions. The frontend is a React + Vite application in src/ai-genius-web."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Automatic Main Deployment (Priority: P1)

As a release owner, I want every change merged to the main branch to automatically build and deploy the frontend so the live web app stays current without manual deployment steps.

**Why this priority**: This is the core delivery path and provides immediate operational value for day-to-day releases.

**Independent Test**: Can be fully tested by pushing a frontend change to main and verifying one workflow run completes build and deploy with a successful status.

**Acceptance Scenarios**:

1. **Given** a commit is pushed to main, **When** the frontend deployment workflow starts, **Then** it runs a build-and-deploy pipeline without manual intervention.
2. **Given** the workflow run starts from a main push, **When** the run finishes successfully, **Then** the latest frontend build is published to the configured Static Web App.

---

### User Story 2 - Manual Environment Deployment (Priority: P2)

As a release owner, I want to manually trigger a deployment run so I can redeploy when needed outside of a code push.

**Why this priority**: Manual dispatch supports operational recovery and controlled redeployments.

**Independent Test**: Can be tested by triggering the workflow manually and confirming a full build and deploy run completes successfully.

**Acceptance Scenarios**:

1. **Given** no new commit is pushed, **When** an authorized user starts the workflow manually, **Then** a deployment run executes using the same build-and-deploy path.

---

### User Story 3 - Predictable, Non-Overlapping Runs (Priority: P3)

As an operations engineer, I want deployment runs to follow the same environment and concurrency controls as infrastructure deployment so parallel runs do not conflict.

**Why this priority**: Consistent controls reduce race conditions and deployment confusion.

**Independent Test**: Can be tested by triggering overlapping runs and confirming concurrency behavior matches the infra workflow pattern.

**Acceptance Scenarios**:

1. **Given** a deployment run is already in progress for the same branch context, **When** another run is triggered, **Then** concurrency handling follows the established infra workflow behavior.
2. **Given** a run is triggered from either main push or manual dispatch, **When** environment resolution is applied, **Then** the selected environment behavior matches the infra workflow standard.

---

### Edge Cases

- What happens when frontend dependencies fail to install during the workflow run?
- How does the workflow handle a successful build but failed deployment to Static Web Apps?
- What happens when required deployment secrets are missing or invalid at runtime?
- How does the workflow behave when two deployment triggers happen close together?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST provide a GitHub Actions workflow file named `002-deploy-web.yml` for frontend deployment.
- **FR-002**: System MUST trigger the workflow on every push to the main branch.
- **FR-003**: System MUST allow manual execution through workflow dispatch.
- **FR-004**: System MUST apply the same environment selection behavior used by the infrastructure deployment workflow.
- **FR-005**: System MUST apply the same concurrency strategy used by the infrastructure deployment workflow.
- **FR-006**: System MUST install frontend dependencies before building the application.
- **FR-007**: System MUST build the frontend application and produce deployable output.
- **FR-008**: System MUST deploy the built frontend output to Azure Static Web Apps.
- **FR-009**: System MUST authenticate to Azure for the deployment workflow using repository secret credentials.
- **FR-010**: System MUST use a Static Web Apps deployment token secret for publishing.
- **FR-011**: System MUST fail the workflow run when build or deployment steps do not complete successfully.
- **FR-012**: System MUST expose clear workflow run status in GitHub Actions for both automatic and manual runs.

### Key Entities *(include if feature involves data)*

- **Frontend Deployment Workflow**: A CI/CD process definition that governs trigger sources, environment resolution, concurrency controls, build execution, and publish behavior.
- **Deployment Run**: A single workflow execution instance containing trigger context, step outcomes, and final status.
- **Deployment Credentials**: Repository-managed secrets required for Azure authentication and Static Web App publishing authorization.
- **Frontend Build Output**: The deployable static artifact generated from the frontend source.

## Assumptions

- The existing infrastructure workflow already defines the project standard for environment and concurrency behavior.
- Required secrets are managed in repository settings before deployment runs are executed.
- Frontend source of truth remains the existing application under src/ai-genius-web.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: 100% of pushes to main automatically start one frontend deployment workflow run.
- **SC-002**: 95% or more of valid deployment runs complete successfully end-to-end without manual step retries over a 30-day period.
- **SC-003**: 100% of manual dispatch attempts by authorized users can start a deployment run within 1 minute of request.
- **SC-004**: For overlapping trigger events in the same branch context, run behavior consistently follows the defined concurrency policy with no conflicting parallel deployments.
