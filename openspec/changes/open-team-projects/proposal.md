# Proposal: Open Team Projects Feature

## Change ID
`open-team-projects`

## Summary
Remove the enterprise license gate from the team projects feature, making it available to all n8n users without any license restrictions. Team projects will be unlimited by default for all instances.

## Motivation
Team projects provide essential collaboration capabilities that benefit all n8n users. By removing the license restriction, community and self-hosted users can:
- Organize workflows and credentials into logical projects
- Manage access and permissions at the project level
- Better structure their automation assets

## Current Behavior
- `getMaxTeamProjects()` returns `0` by default (disabled)
- `getTeamProjectLimit()` returns `0` by default (disabled)
- Frontend checks `teamProjectsLimit !== 0` to determine if feature is enabled
- Creating a team project throws `TeamProjectOverQuotaError` when limit is reached

## Proposed Behavior
- `getMaxTeamProjects()` returns `UNLIMITED_LICENSE_QUOTA` (-1) by default
- `getTeamProjectLimit()` returns `UNLIMITED_LICENSE_QUOTA` (-1) by default
- Frontend will show team projects as enabled with unlimited quota
- Licensed instances can still have their quota enforced via license

## Scope
- **Backend**: Change default return values in license state queries
- **Frontend**: No changes required (already handles unlimited correctly)
- **Database**: No schema changes
- **API**: No endpoint changes

## Impact Analysis

### Files to Modify
1. `packages/@n8n/backend-common/src/license-state.ts` - Change default in `getMaxTeamProjects()`
2. `packages/cli/src/license.ts` - Change default in `getTeamProjectLimit()`

### Affected Components
- License state service
- Project service (no code changes needed - already handles unlimited)
- Frontend projects store (no code changes needed - already handles unlimited)
- Frontend settings display

### Breaking Changes
None. This is an additive change that enables functionality rather than removing it.

### Risks
- Low risk: Licensed instances will continue to have their quotas enforced
- The existing quota enforcement logic is preserved for cases where a license specifies a limit

## Success Criteria
1. Unlicensed instances can create unlimited team projects
2. Licensed instances with quotas still have their limits enforced
3. All existing tests pass
4. Frontend correctly shows team projects as available
