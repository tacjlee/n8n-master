# Tasks: Open Team Projects Feature

## Implementation Tasks

### 1. Update License State Default
**File:** `packages/@n8n/backend-common/src/license-state.ts`
**Change:** Modify `getMaxTeamProjects()` to return `UNLIMITED_LICENSE_QUOTA` instead of `0` as the default value.

```typescript
// Before
getMaxTeamProjects() {
    return this.getValue('quota:maxTeamProjects') ?? 0;
}

// After
getMaxTeamProjects() {
    return this.getValue('quota:maxTeamProjects') ?? UNLIMITED_LICENSE_QUOTA;
}
```

**Validation:** Unit tests for license state should be updated to reflect new default.

---

### 2. Update License Service Default
**File:** `packages/cli/src/license.ts`
**Change:** Modify `getTeamProjectLimit()` to return `UNLIMITED_LICENSE_QUOTA` instead of `0` as the default value.

```typescript
// Before
getTeamProjectLimit() {
    return this.getValue(LICENSE_QUOTAS.TEAM_PROJECT_LIMIT) ?? 0;
}

// After
getTeamProjectLimit() {
    return this.getValue(LICENSE_QUOTAS.TEAM_PROJECT_LIMIT) ?? UNLIMITED_LICENSE_QUOTA;
}
```

**Validation:** Unit tests for license service should be updated to reflect new default.

---

### 3. Update E2E Controller Default
**File:** `packages/cli/src/controllers/e2e.controller.ts`
**Change:** Update the default value in `numericFeaturesDefaults` for `TEAM_PROJECT_LIMIT` from `0` to `-1` (unlimited).

**Validation:** E2E tests should continue to work with the new default.

---

### 4. Update Test Mocks and Fixtures
**Files:**
- `packages/cli/src/services/__tests__/frontend.service.test.ts`
- `packages/cli/test/integration/workflows/workflow-sharing.service.test.ts`
- Various frontend test files that mock `teamProjectsLimit`

**Change:** Update mock values to reflect the new unlimited default where appropriate.

**Validation:** All tests pass.

---

### 5. Run Full Test Suite
**Command:** `pnpm test:affected`

**Validation:**
- All unit tests pass
- All integration tests pass
- TypeScript type checking passes (`pnpm typecheck`)
- Linting passes (`pnpm lint`)

---

## Verification Steps

1. Start n8n without a license
2. Navigate to Projects in the UI
3. Verify "Create Project" button is visible and enabled
4. Create a new team project
5. Verify no quota error is shown
6. Create multiple team projects to confirm unlimited behavior

## Dependencies
- No external dependencies
- Tasks can be completed in order listed

## Parallelizable Work
- Tasks 1 and 2 can be done in parallel (independent file changes)
- Task 3 can be done in parallel with 1 and 2
- Task 4 should be done after 1-3 to ensure test expectations match new behavior
- Task 5 must be done last
