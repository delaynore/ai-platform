# AI Platform

The platform connects company employees, their agents, and projects.

## Language

**Employee**:
A person in the company represented in the platform, including before they accept an invitation.
_Avoid_: User, developer when referring to platform access.

**Administrator**:
An employee responsible for invitations, project management, employee access, and employee deactivation.
_Avoid_: Owner, manager when referring to this role.

**Project**:
A group of company repositories and work instructions. Membership provides access to project information; repository access requires valid authorization in the code host.
_Avoid_: Repository when referring to the whole project.

**Employee agent**:
An agent connected by an employee whose access to company data is limited by that employee's access.
_Avoid_: Company agent, platform agent.

**Agent authorization**:
An employee's explicit permission for their agent to access platform information on their behalf, limited to one current authorization per employee.
_Avoid_: Network connection, project membership.

**Invitation**:
A time-limited, single-use offer for an identified person to join the platform as an employee.
_Avoid_: Access grant when referring to joining the platform.

**Invitation recipient**:
A person addressed by an invitation, identified before identity verification by their normalized email address.
_Avoid_: Verified employee when the person has not yet accepted an invitation.

**Project membership**:
An assignment of an employee to a project that permits project information access when the employee has active platform access.
_Avoid_: Repository permission, GitHub access.

**Repository entry**:
A project's record identifying a GitHub repository. Different projects can hold independent entries for the same repository.
_Avoid_: Repository when referring to the platform record rather than the code host resource.

**Project instructions**:
Work instructions belonging to a project, which employees and their agents read to prepare the local environment.
_Avoid_: Shared skill, company instructions.

**Project onboarding**:
An employee's introduction to a project with their agent's help, covering project instructions, local environment preparation, and repository access checks.
_Avoid_: Company agent, cloud execution when referring to this process.
