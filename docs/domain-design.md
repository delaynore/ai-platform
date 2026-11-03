# First-version domain design

This document records agreed domain rules and module design for the [first version](first-version.md).
The [glossary](../DOMAIN.md) defines domain terms.
Four modules, their responsibilities, domain rules, and core interactions are recorded below.
This document includes the agreed boundary-case behavior.

## Scope

The platform serves one company and one selected development agent.
Project information access and code host access are separate.
GitHub account linking and automatic repository permission changes remain deferred.
The platform does not execute agent commands.

## Agreed domain rules

### Employee invitation and project assignment

The platform records an employee before invitation acceptance.
The administrator can assign projects to that employee before acceptance.
The invitation refers to that employee record.
The employee obtains platform access only after identity confirmation and invitation acceptance.
Expired, cancelled, and previously accepted invitations cannot grant access.

An employee is pending, active, or deactivated.
A pending employee has not accepted an invitation and has no platform access.
Invitation expiry or cancellation preserves the employee record and project memberships.
The administrator can issue a replacement invitation; doing so immediately invalidates the previous invitation.
Accepting a valid invitation after identity confirmation activates the employee.
Deactivation stops all platform access for that employee and their agent.
Reactivating a deactivated employee is outside this version.
The first administrator is created during initial setup without an invitation.

Each verified identity corresponds to at most one employee record.
The person accepting an invitation must match its intended recipient.
Invitations to active or deactivated employees are rejected.
Concurrent attempts to accept one invitation can activate the employee only once.
The identity verification method and invitation lifetime remain undecided.

Employees uses the normalized recipient email address as a unique key before identity verification.
Inviting the same address again reuses its pending employee record, replaces the invitation, and preserves all project memberships.
Concurrent invitation requests cannot create multiple employee records for that recipient key.
Different email addresses represent different recipients until identity verification establishes otherwise.
If acceptance identifies a person already linked to another employee record, Employees rejects acceptance.
The platform does not automatically merge employee records or their project memberships.

Deactivation preserves existing project memberships but permanently ends the validity of the employee's previous agent authorization.
Access rejects new memberships for deactivated employees; the administrator can remove their previous memberships.
The administrator cannot deactivate themselves through ordinary employee management.
Recovery after administrator access loss belongs to later operational planning.

### Administrator and employee agent access

The administrator manages every project and reads its information without project membership.
The administrator's employee agent can read only projects explicitly assigned to the administrator.
An employee agent does not inherit administrative powers.
Every employee agent's access remains limited by its employee's active platform access and project memberships.

### Information visibility

The employee directory, invitations, and company-wide membership management are restricted to the administrator.
An employee can see colleagues assigned to the same project.
The employee and their agent receive the names of employees who are active and assigned when the colleague list is formed.
Pending and deactivated employees are excluded when forming this list; their administrative records remain visible only to the administrator.
The colleague list does not reveal other projects, invitations, or agent authorizations.
Access determines the permitted colleague list from memberships and current employee facts supplied by Employees.
Projects includes this permitted list in project information; there is no employee-facing company directory.
Colleague activity is not checked again immediately before sending the response.
If a colleague is deactivated after list formation, that response may still contain their name.
A newly formed list excludes that colleague; this exception never preserves the deactivated employee's own access.
The colleague list and project contents do not require one common snapshot across all modules.
Project lists contain only projects the caller may access.
Requests for inaccessible projects do not reveal whether those projects exist.
An employee can see the state of their own agent authorization.
Access enforces these visibility rules.

### Project membership changes

Access stores at most one membership for each employee and project pair.
Repeating an assignment preserves one membership; repeating a removal leaves membership absent.
Removing membership does not invalidate the employee's agent authorization for other projects.
Reassigning a project to an active employee restores access for their currently authorized agent without reconnection.
Removing the administrator's membership stops their agent's project access but preserves the administrator's access when acting as a human.

### Agent authorization

An active employee explicitly authorizes their agent to access the platform.
Each employee can have only one current agent authorization.
This limit concerns access permission, not concurrent network connections.
Replacing an authorization requires explicit employee consent and immediately invalidates the previous authorization.
Disconnecting the agent immediately stops access through that authorization while preserving the employee's own access.
The supported agent and connection protocol remain undecided.

### Repository entries

A project owns its repository entries.
Different projects can contain independent entries identifying the same GitHub repository.
There is no shared repository catalog in this version.
Removing an entry from one project does not change entries in another project.
An employee can still obtain repository information through another project they can access.
Repository entry changes never change code host permissions.
Every listed repository is required for local environment preparation; there is no optional repository flag.
Repository entry addresses must be unique within a project after normalization; Projects rejects matching normalized addresses.
This guarantee concerns addresses, not the identity of the actual GitHub repository.
Without GitHub queries, Projects does not guarantee recognition of historical addresses, redirects, or aliases for the same repository.
Projects validates the repository address without querying GitHub to confirm repository existence or code access.
Repository entries contain no credentials.
If a repository moves, is renamed, or disappears, the agent reports the access problem to the employee.
The administrator corrects affected entries manually; automatic discovery and synchronization remain deferred.

### Project information and completeness

A project may initially have no repository entries or instructions.
Assigned employees can read its current information even while it is incomplete.
There is no publication state.
Environment preparation requires at least one repository entry and nonempty instructions.
The agent reports missing information instead of reporting successful preparation.
The administrator can update the project after assigning employees.

Each project read returns its instructions and complete repository list as one consistent view.
The view represents one actual project state during the read; it cannot mix values from different states.
Adding a repository and later updating instructions are separate operations; the intermediate state is valid and may be read.
There is no operation for jointly publishing related instruction and repository changes.
The next read receives current information; the platform does not push updates to a running agent.
The agent prepares the environment using the view it received.
There is no separate version history or platform tracking of local execution.

### Local environment preparation

The employee agent helps the employee understand project instructions and prepare to start local development.
The platform supplies permitted project information and instructions.
The employee agent prepares the local environment, checks repository access, and reports results to the employee.
The platform does not store preparation state or introduce a preparation run concept.
The agent cannot report completion while any listed repository remains inaccessible.
Successful preparation also requires successful completion of every mandatory instruction step and its required readiness checks.
Failed dependency installation or another mandatory step prevents a success report even when every repository is accessible.
If instructions omit readiness checks, the agent reports that verification is incomplete instead of claiming verified environment readiness.
The employee resolves code host access outside the platform.
The agent acts locally according to the selected agent's rules and reports obstacles to the employee.
Read-only platform access does not prohibit local environment preparation.
Development with the agent may continue outside the platform; development management scenarios remain outside this version.

## Modules

A module groups behavior behind an interface; it does not imply a separate deployment.
Interfaces must hide domain checks rather than require callers to assemble access rules.

| Module | Owned concepts | Responsibilities |
| --- | --- | --- |
| Employees | Employee, Administrator, Invitation | Employee identity and lifecycle, administrator status, invitations, and current employee facts. |
| Access | Project membership | Membership assignment and removal; combined authorization decisions for platform and project operations. |
| Projects | Project, Repository entry, Project instructions | Project management and consistent project information reads authorized by Access. |
| Agent Access | Agent authorization | Explicit authorization, replacement, disconnection, and current agent authorization facts. |

Access combines current employee facts, agent authorization facts, and project memberships behind its interface.
Projects asks Access to authorize operations instead of reproducing access rules.
An Access decision covers a specific operation and target, not an unlimited permission for future requests.
No module grants or revokes code host permissions.

### Access capabilities

An active employee can use the platform, read assigned projects, and manage their own agent authorization.
The administrator can additionally manage employees and all projects.
An employee agent reads only projects assigned to its employee, including when that employee is the administrator.
Project membership covers all repository information and instructions; separate permissions within a project are deferred.
There are no configurable permission sets or additional roles in this version.
There is no separate platform access ban for an active employee; deactivation stops their platform access.
Only a human employee can authorize, replace, or disconnect their own agent.
An employee agent cannot change platform information, memberships, or authorizations, including its own authorization.

| Capability | Active employee | Administrator acting as a human | Employee agent with current authorization |
| --- | --- | --- | --- |
| Read project information and active colleague names | Assigned projects | All projects | Projects assigned to its employee |
| Create projects; edit instructions and repository entries | Denied | Allowed | Denied |
| Invite or deactivate employees | Denied | Allowed, except self-deactivation | Denied |
| Assign or remove project membership | Denied | Allowed | Denied |
| Read the employee directory and administrative records | Denied | Allowed | Denied |
| Manage own agent authorization | Allowed | Allowed | Denied |

Pending and deactivated employees cannot use these capabilities.
Invitation acceptance remains the limited entry operation for the verified recipient of a valid invitation.

### Interfaces and interactions

Employees identifies the employee making a human request and provides their current state and administrator status.
Agent Access identifies an employee from an agent authorization and checks its validity using its own state and Employees.
Access evaluates the verified caller, operation, and target using current employee and agent facts and its own memberships.
Access distinguishes human requests from agent requests and does not inherit administrator powers for an agent.
Client-supplied employee identifiers, project identifiers, or administrator claims do not prove authority.
Projects obtains an Access decision before each operation and before releasing project information.
All project information passes through the Projects interface.
Employees and Agent Access do not read project contents to decide access.

| Interaction | Rule owner | Required collaboration |
| --- | --- | --- |
| Accept an invitation | Employees | Verify recipient identity, invitation validity, and employee eligibility before activation. |
| Assign project membership | Access | Verify authority and employee eligibility through Employees, confirm project existence through Projects, then store membership. |
| Authorize or disconnect an agent | Agent Access | Access determines whether the caller may perform the operation; Agent Access controls authorization validity. |
| Manage project information | Projects | Access confirms authority for the operation; Projects changes its own information. |
| Read project information as a human | Projects | Access checks active employee state and administrator status or project membership. |
| Read project information as an agent | Projects | Access checks current agent authorization, active employee state, and membership without administrator inheritance. |
| Include colleagues in project information | Projects | Access selects members active at list formation using memberships and Employees facts; Projects includes only their names. |
| Remove project membership | Access | Remove membership after checking authority; deny subsequent employee and agent reads, except reads by the administrator acting as a human. |
| Deactivate an employee | Employees | Access confirms administrative authority; Employees rejects administrator self-deactivation and changes employee state. |

Caller identification does not replace a current access check before information release.
Access owns the obligation to reject assignments to nonexistent projects; Projects owns the authoritative project existence check.
An arbitrary client-supplied project identifier is not proof that a project exists.
Every protected operation requires confirmed current authority, including reads, project changes, invitations, membership changes, and agent authorization changes.
If authority cannot be confirmed, the operation must not disclose protected information or apply its requested state change.
Access and Agent Access must treat a deactivated employee's authorization as invalid, without requiring membership deletion in Access.
These collaborations describe domain obligations, not a transport protocol or implementation sequence.

### Access revocation and concurrent requests

Successful revocation means the platform has stopped handing new response data to the network transport under the lost authority.
A prepared but unsent response must not be sent under that authority.
For a partially sent response, the platform stops handing over its remaining data before reporting successful revocation.
Data already handed to the network transport may arrive later and cannot be recalled.
After successful revocation, an unfinished protected operation cannot apply changes unless current authority still permits them.
Changes already applied remain in effect; revocation does not undo them.
Removing administrator membership preserves human administrator authority but removes their agent's project authority.
An existing network connection does not preserve previous rights.
If a module cannot confirm current authority, the platform denies the protected operation without applying its requested changes.
Implementation planning must select a mechanism that enforces this ordering across collaborating modules.
Downloaded information and code host permissions remain outside the platform's revocation capability.

### Deferred integrations

Integrations remains a planned module without an implementation in this version.
Future external connections belong there; their interfaces are not designed in this document.
Projects owns manually entered repository information.
The local employee agent checks GitHub access independently of the platform.

## Verification scenarios for implementation

These scenarios describe future checks through module interfaces; they are not executed tests.

| Scenario | Expected behavior |
| --- | --- |
| Assign a project before invitation acceptance | Membership exists, but the pending employee and agent cannot access project information. |
| Assign a nonexistent project identifier | Access rejects the assignment using the authoritative Projects existence check; no membership is stored. |
| Accept an expired, cancelled, replaced, or used invitation | No activation or access; concurrent acceptance activates at most once. |
| Invite the same normalized email address twice, including concurrently | Reuse one pending employee record and preserve memberships; the latest replacement invalidates the prior invitation. |
| Accept invitations for different addresses that verify as the same person | Reject acceptance into a second employee record; do not merge records or memberships. |
| Read an unrelated project using a known identifier | No information or confirmation that the project exists. |
| Connect the administrator's agent without assigning projects | The agent cannot inherit access to every project. |
| Read project colleagues | Show only names of assigned employees active at list formation; disclose no other projects or invitation information. |
| Deactivate a colleague between list formation and response transmission | The prepared list may contain their name; a newly formed list excludes it. Their own access still stops. |
| Revoke access after a response is prepared but before sending | Do not send the prepared response under the lost authority. |
| Remove an ordinary employee's membership during partial response transmission | Stop sending remaining data before reporting successful removal; previously handed-off data may still arrive. |
| Revoke authority during a protected write | After successful revocation, apply no pending change under lost authority; retain changes already applied. |
| Remove the administrator's project membership | Their agent loses project access; the administrator acting as a human retains access. |
| Repeat membership changes, then reassign the project | No duplicate membership; a current agent authorization works after reassignment. |
| Disconnect or replace an agent authorization | The previous authorization cannot obtain further information; the employee retains access. |
| Deactivate an employee | All employee and agent platform access stops; retained memberships do not restore access. |
| Read between separate repository and instruction edits | The intermediate actual project state is permitted; one read must not mix values from different states. |
| Add matching normalized repository addresses | Reject the duplicate within the project; allow independent entries across projects. |
| Add different addresses that GitHub redirects to one repository | Platform uniqueness does not promise detection of the shared GitHub repository identity. |
| Remove a repository entry shared across projects | Other projects retain their independent entries; GitHub permissions do not change. |
| Prepare an incomplete project or encounter an inaccessible repository | The agent reports the obstacle and does not report successful preparation. |
| Fail a mandatory preparation step despite repository access | The agent reports failure and does not report successful preparation. |
| Prepare using instructions without readiness checks | The agent reports the verification limitation rather than claiming verified readiness. |
| Fail to confirm authority for a protected write | No project, invitation, membership, employee, or agent authorization change is applied. |

## Implementation planning and review status

Module ownership, domain rules, and the reviewed boundary cases are agreed.
This agreement is not implementation verification or authorization to implement or publish the platform.
Technology stack, application interface, identity verification method, invitation lifetime, agent protocol, and supported agent remain implementation planning choices.
Email and repository address normalization rules and mechanisms enforcing consistency and revocation remain implementation planning work.
Those mechanisms must preserve the observable guarantees defined here; their behavior has not been tested.
Administrator access recovery also remains an operational planning topic.
No implementation, CI execution, deployment, or publication is part of this documentation change.

## Decision records

[ADR 0001](adr/0001-centralize-platform-access-rules.md) records why Access owns memberships and combined authorization rules instead of Projects.
