# AI Platform

A platform for employees and agents to work together. It will bring company projects, repositories, instructions, and access management into one place.

## Project status

The project is being prepared for development. The repository contains customer requirements, agent instructions, and basic Git configuration.

The technology stack, architecture, and agent connection method have not been selected. Application code, build tooling, tests, and CI are not available yet.

## First version

The first version will help developers join a project:

- Employees join the platform by invitation.
- Projects group GitHub repositories and work instructions.
- Employees and their agents can access only authorized projects.
- Agents prepare the development environment from project instructions and check repository access.
- When an employee leaves, the platform revokes their access and stops providing company data to their agent.
- A separate integrations module connects external services. GitHub will be the first integration.

See [customer-requirements.md](customer-requirements.md) for detailed requirements and possible future directions.

The agreed [first-version scope](docs/first-version.md) defers GitHub account linking and repository permission management.
This version revokes access to platform information; repository access remains managed outside the platform.

## Getting started

Clone the repository:

```sh
git clone https://github.com/delaynore/ai-platform.git
cd ai-platform
```

Read the [customer requirements](customer-requirements.md) before planning changes.

Setup, run, and verification commands will be added here with the application code.

## Working with the repository

- The default branch is `master`.
- Prepare changes on separate branches and submit pull requests.
- Follow the [review and merge workflow](docs/agents/pull-requests.md).

## Agent instructions

[AGENTS.md](AGENTS.md) defines the rules for agents working in this repository.

- [Domain documentation](docs/agents/domain.md) covers project terms and architectural decisions.
- [Issue tracker](docs/agents/issue-tracker.md) describes how to work with GitHub Issues.
- [Triage labels](docs/agents/triage-labels.md) defines issue states.
- [PR workflow](docs/agents/pull-requests.md) covers the agent account, review, and merging.

The [.agents/skills](.agents/skills/) directory contains reusable instructions for development, research, and review.
