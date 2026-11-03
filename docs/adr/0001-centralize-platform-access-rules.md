---
status: accepted
---

# Centralize platform access rules in Access

Platform access depends on employee state, project membership, agent authorization, and whether a human or an agent makes the request.
Access owns project memberships and combines these facts to authorize protected operations across the platform.
This keeps access policy behind one interface and prevents other modules from independently assembling rules.

## Considered alternatives

- Keep memberships and combined checks in Projects. This keeps project assignment near project contents but couples platform-wide authorization to Projects.
- Give Access ownership of memberships and combined decisions. This adds collaboration between modules but gives access policy one owner.

We chose the second option so project reads, administrative changes, and agent operations use the same authorization rules.

## Consequences

Employees owns employee identity, lifecycle, administrator status, and invitations.
Agent Access owns agent authorization, replacement, and disconnection.
Projects owns project contents and supplies the authoritative project existence check before Access stores a membership.
These modules supply facts or enforce Access decisions without taking ownership of its policy.

An Access decision applies to a specific operation and target; it does not preserve authority after access is revoked.
Removing administrator membership stops their agent's project access while preserving access for the administrator acting as a human.
Every protected operation must reject unconfirmed authority and obey the agreed revocation guarantees.
Cross-module coordination must preserve those guarantees; the implementation mechanism remains undecided.

Moving this policy later would require changing membership ownership and authorization interactions across all four modules.
This decision introduces no configurable permissions, additional roles, separate deployment requirement, or code host permission management.
The [domain design](../domain-design.md) defines detailed behavior and verification scenarios within the [first-version scope](../first-version.md).
