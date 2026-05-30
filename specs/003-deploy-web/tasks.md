# Tasks: Deploy AI Genius Web Frontend

**Input**: Design documents from `/specs/003-deploy-web/`
**Prerequisites**: plan.md, spec.md, research.md, data-model.md, contracts/workflow-interface.md, quickstart.md

**Tests**: No dedicated automated test files are required by this feature spec. Validation is done through GitHub Actions run outcomes and quickstart verification steps.

**Organization**: Tasks are grouped by user story so each story can be implemented and validated independently.

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Confirm baseline workflow references and operational documentation before implementation changes.

- [X] T001 Review deployment baseline and capture infra workflow reference in specs/003-deploy-web/plan.md
- [X] T002 Confirm required repository secrets and environment variables in specs/003-deploy-web/quickstart.md
- [X] T003 [P] Verify frontend build commands and artifact path assumptions in src/ai-genius-web/package.json

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Establish shared workflow contract and core deployment structure that all user stories depend on.

- [X] T004 Define trigger, job skeleton, and deployment contract baseline in .github/workflows/002-deploy-web.yml
- [X] T005 [P] Align workflow input/output contract details in specs/003-deploy-web/contracts/workflow-interface.md
- [X] T006 [P] Align operational entity/state model for workflow behavior in specs/003-deploy-web/data-model.md
- [X] T007 [P] Record final action/version decisions and rationale in specs/003-deploy-web/research.md

**Checkpoint**: Foundation complete. User stories can now proceed independently.

---

## Phase 3: User Story 1 - Automatic Main Deployment (Priority: P1) 🎯 MVP

**Goal**: Ensure every push to `main` builds and deploys the frontend automatically.

**Independent Test**: Push a frontend change to `main` and verify one successful run of `.github/workflows/002-deploy-web.yml` completes build and deploy.

### Implementation for User Story 1

- [X] T008 [US1] Configure `push` trigger for `main` in .github/workflows/002-deploy-web.yml
- [X] T009 [US1] Add Node.js setup and dependency install steps in .github/workflows/002-deploy-web.yml
- [X] T010 [US1] Add frontend build step with `VITE_API_URL` environment injection in .github/workflows/002-deploy-web.yml
- [X] T011 [US1] Add Azure login and Static Web Apps deploy steps using required secrets in .github/workflows/002-deploy-web.yml
- [X] T012 [P] [US1] Document automatic push deployment verification flow in specs/003-deploy-web/quickstart.md

**Checkpoint**: User Story 1 is independently deployable and verifiable.

---

## Phase 4: User Story 2 - Manual Environment Deployment (Priority: P2)

**Goal**: Allow authorized users to manually trigger frontend deployment without new code pushes.

**Independent Test**: Run workflow manually from GitHub Actions and confirm successful build + deploy path.

### Implementation for User Story 2

- [X] T013 [US2] Add `workflow_dispatch` input definition for environment selection in .github/workflows/002-deploy-web.yml
- [X] T014 [US2] Apply selected environment to the deployment job context in .github/workflows/002-deploy-web.yml
- [X] T015 [P] [US2] Document manual dispatch steps and expected result in specs/003-deploy-web/quickstart.md
- [X] T016 [P] [US2] Update manual input contract details for `workflow_dispatch` in specs/003-deploy-web/contracts/workflow-interface.md

**Checkpoint**: User Story 2 can be executed and validated independently of other stories.

---

## Phase 5: User Story 3 - Predictable, Non-Overlapping Runs (Priority: P3)

**Goal**: Match infra workflow environment/concurrency behavior and prevent conflicting parallel runs.

**Independent Test**: Trigger overlapping runs and confirm concurrency cancellation/grouping and environment behavior match infra workflow conventions.

### Implementation for User Story 3

- [X] T017 [US3] Implement infra-aligned concurrency group and cancel-in-progress policy in .github/workflows/002-deploy-web.yml
- [X] T018 [US3] Add deployment summary output for run traceability in .github/workflows/002-deploy-web.yml
- [X] T019 [P] [US3] Document concurrency and environment behavior checks in specs/003-deploy-web/quickstart.md
- [X] T020 [P] [US3] Align expected behavior statements for concurrency/environment in specs/003-deploy-web/contracts/workflow-interface.md

**Checkpoint**: User Story 3 behavior is independently testable via controlled overlapping runs.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Final consistency, validation, and readiness updates across all stories.

- [X] T021 [P] Sync final feature summary and implementation boundaries in specs/003-deploy-web/plan.md
- [X] T022 Validate end-to-end quickstart against workflow behavior and update notes in specs/003-deploy-web/quickstart.md
- [X] T023 Validate YAML syntax and action references for .github/workflows/002-deploy-web.yml

---

## Dependencies & Execution Order

### Phase Dependencies

- Setup (Phase 1): No dependencies.
- Foundational (Phase 2): Depends on Setup completion; blocks all user stories.
- User Story phases (Phase 3-5): Depend on Foundational completion.
- Polish (Phase 6): Depends on completion of selected user stories.

### User Story Dependencies

- User Story 1 (P1): Can start after Phase 2.
- User Story 2 (P2): Can start after Phase 2.
- User Story 3 (P3): Can start after Phase 2.

### Recommended Delivery Order

- Complete MVP first: US1.
- Then add operational flexibility: US2.
- Then add run control and consistency hardening: US3.

---

## Parallel Opportunities

- Phase 1: T003 can run in parallel with T001-T002.
- Phase 2: T005, T006, and T007 can run in parallel after T004 starts the baseline.
- US1: T012 can run in parallel with T008-T011.
- US2: T015 and T016 can run in parallel with T013-T014.
- US3: T019 and T020 can run in parallel with T017-T018.
- Polish: T021 can run in parallel with T022 once user stories are complete.

---

## Parallel Example: User Story 1

```bash
# Parallel documentation while workflow implementation is in progress:
Task: T012 Document automatic push deployment verification flow in specs/003-deploy-web/quickstart.md

# Core workflow implementation sequence:
Task: T008 Configure push trigger for main in .github/workflows/002-deploy-web.yml
Task: T009 Add Node.js setup and dependency install steps in .github/workflows/002-deploy-web.yml
Task: T010 Add frontend build step with VITE_API_URL environment injection in .github/workflows/002-deploy-web.yml
Task: T011 Add Azure login and Static Web Apps deploy steps using required secrets in .github/workflows/002-deploy-web.yml
```

## Parallel Example: User Story 2

```bash
# Implement manual dispatch path:
Task: T013 Add workflow_dispatch input definition for environment selection in .github/workflows/002-deploy-web.yml
Task: T014 Apply selected environment to the deployment job context in .github/workflows/002-deploy-web.yml

# Document contract and runbook in parallel:
Task: T015 Document manual dispatch steps and expected result in specs/003-deploy-web/quickstart.md
Task: T016 Update manual input contract details for workflow_dispatch in specs/003-deploy-web/contracts/workflow-interface.md
```

## Parallel Example: User Story 3

```bash
# Workflow behavior hardening:
Task: T017 Implement infra-aligned concurrency policy in .github/workflows/002-deploy-web.yml
Task: T018 Add deployment summary output for run traceability in .github/workflows/002-deploy-web.yml

# Contract/runbook verification in parallel:
Task: T019 Document concurrency and environment behavior checks in specs/003-deploy-web/quickstart.md
Task: T020 Align expected behavior statements in specs/003-deploy-web/contracts/workflow-interface.md
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1 and Phase 2.
2. Complete US1 tasks (T008-T012).
3. Validate by push-trigger deployment success.
4. Demo/deploy MVP.

### Incremental Delivery

1. Deliver US1 for automatic deployment baseline.
2. Deliver US2 for manual operational control.
3. Deliver US3 for predictable concurrency and environment behavior.
4. Complete polish tasks and perform final validation.

### Parallel Team Strategy

1. One engineer owns workflow YAML tasks in .github/workflows/002-deploy-web.yml.
2. Another engineer updates quickstart and contract docs in specs/003-deploy-web/.
3. Merge per story checkpoint to keep each increment independently testable.
