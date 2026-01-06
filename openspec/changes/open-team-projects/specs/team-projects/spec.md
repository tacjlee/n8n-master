# Spec Delta: Team Projects Availability

## Capability
Team Projects - Multi-user project collaboration and organization

## MODIFIED Requirements

### Requirement: Team Project Default Availability
**Previous:** Team projects are disabled by default (quota = 0) and require an enterprise license to enable.

**New:** Team projects are enabled and unlimited by default (quota = -1) for all instances. Licensed instances may still have specific quotas enforced.

#### Scenario: Unlicensed instance creates team project
**Given** an n8n instance without an enterprise license
**When** a user with project:create scope attempts to create a team project
**Then** the team project is created successfully
**And** no quota error is raised

#### Scenario: Unlicensed instance creates multiple team projects
**Given** an n8n instance without an enterprise license
**And** the instance already has 10 team projects
**When** a user with project:create scope attempts to create another team project
**Then** the team project is created successfully
**And** no quota error is raised

#### Scenario: Licensed instance with quota creates team project within limit
**Given** an n8n instance with an enterprise license specifying quota:maxTeamProjects = 5
**And** the instance has 3 team projects
**When** a user with project:create scope attempts to create a team project
**Then** the team project is created successfully

#### Scenario: Licensed instance with quota exceeds limit
**Given** an n8n instance with an enterprise license specifying quota:maxTeamProjects = 5
**And** the instance has 5 team projects
**When** a user with project:create scope attempts to create a team project
**Then** a TeamProjectOverQuotaError is raised
**And** the team project is not created

#### Scenario: Licensed instance with unlimited quota
**Given** an n8n instance with an enterprise license specifying quota:maxTeamProjects = -1
**When** a user with project:create scope attempts to create team projects
**Then** team projects are created successfully without limit

---

### Requirement: Frontend Team Projects Feature Flag
**Previous:** `isTeamProjectFeatureEnabled` returns `false` when `teamProjectsLimit === 0`.

**New:** `isTeamProjectFeatureEnabled` returns `true` by default since `teamProjectsLimit` defaults to `-1` (unlimited).

#### Scenario: Frontend shows team projects UI for unlicensed instance
**Given** an n8n instance without an enterprise license
**When** a user views the projects navigation
**Then** the "Create Project" option is visible
**And** the user can access team project creation UI

#### Scenario: Frontend hides team projects UI when explicitly disabled
**Given** an n8n instance with a license setting quota:maxTeamProjects = 0
**When** a user views the projects navigation
**Then** the "Create Project" option is hidden or disabled

---

## Related Capabilities
- Project Roles and Permissions (unchanged)
- Workflow Sharing (unchanged)
- Credential Sharing (unchanged)
