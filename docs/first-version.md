# First version

## Goal

Help an employee join a company project with their existing development agent, prepare a local environment, and check access to its code.

This document records the agreed first-version scope based on the [customer requirements](../customer-requirements.md).
Terms are defined in [DOMAIN.md](../DOMAIN.md).

## Scope boundary

The first version serves one company. It supports an administrator, employees, projects, and one selected development agent.

Project membership grants access to information about all project repositories and to project instructions.
It does not grant access to the code itself. Repository operations require valid access in the code host.

**Agreed deferral:** GitHub account linking and automatic repository access grants and revocations are outside this version.
The customer requirements include this capability; this version deliberately narrows that scope.
An employee must already have repository access or obtain it outside the platform.
Deactivating an employee in this version does not revoke their GitHub access.

## Required scenarios

### Set up the first administrator

The first administrator is created during initial platform setup without an invitation.
Other employees join by invitation. Public registration and management of additional administrators are deferred.

### Invite an employee

The administrator invites an employee. The invitation is time-limited, single-use, and can be cancelled by the administrator.
The invited person must confirm their identity before joining.

Expired, cancelled, or previously used invitations cannot be accepted.
The identity verification method and invitation lifetime will be selected later.

### Manage a project

The administrator creates a project, manually lists its repositories, and creates and updates its work instructions.
Employees and their agents can read these instructions but cannot edit them.

The administrator can add or remove repository entries after employees have joined the project.
Employees and their agents then receive the updated repository information.
These changes do not modify repository permissions in the code host.

### Assign and remove project membership

The administrator assigns an employee to a project. Membership exposes information about every repository and all instructions in that project.
Separate permissions for individual repositories within a project are deferred.

The administrator can remove an employee from one project while retaining their account and other project memberships.
This supports employees moving between projects without deactivating their platform accounts.

### Connect an employee agent

An employee explicitly authorizes their existing development agent to connect to the platform.
The employee can disconnect the agent without deactivating their own account.

The agent receives only the projects, repository information, and instructions available to that employee.
An agent does not gain access to an unauthorized project by knowing its name or identifier.

### Prepare a local environment

The agent uses the permitted project instructions to prepare the environment on the employee's computer and checks access to project repositories.
The platform does not run the agent or execute its commands on platform servers.

If a repository is inaccessible, the agent identifies it and reports the problem to the employee.
The agent must not report environment preparation as complete while a required repository remains inaccessible.
The employee resolves missing repository access outside the platform.

### Revoke platform access

Removing project membership immediately prevents the employee and their agent from obtaining further information about that project.
The restriction applies to an already connected agent as well as to new connections.

Deactivating an employee stops all platform access for the employee and their agent.
Disconnecting an agent stops that agent's platform access while preserving the employee's access.

The platform cannot remove code, instructions, or other data already downloaded to the employee's computer.
Repository access in the code host remains outside this version's revocation boundary.

## Acceptance scenario

1. The administrator creates a project, lists its repositories, and adds instructions.
2. The administrator invites an employee and assigns the project to them.
3. The employee accepts the invitation and explicitly connects their agent.
4. The agent retrieves permitted project information, prepares the local environment, and checks code access.
5. The administrator removes the employee's project membership; the employee and agent lose access to that project's platform information.
6. The administrator deactivates the employee; the employee and agent lose all platform access.

At every step, an unrelated project remains inaccessible to the employee and their agent.
Verification must also cover inaccessible repositories, invalid invitations, agent disconnection, and repository list updates.

## Deferred capabilities

- Multiple companies and isolation between companies.
- Teams, additional roles, and employee requests for project access.
- GitHub account linking and automatic repository access grants and revocations.
- Automatic repository discovery through GitHub.
- Project closure, reopening, and permanent deletion.
- Collaborative editing, approval, and separate version history for project instructions.
- The company agent, documentation search, and a shared skills catalog.
- Model spending tracking, a task tracker, cloud agent execution, and parallel agent orchestration.

## Decisions for implementation planning

The technology stack, application interface, identity verification method, and agent connection protocol remain undecided.
The supported development agent will be selected separately.
This document defines required behavior without prescribing those implementation choices.
