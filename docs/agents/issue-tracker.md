# Issue tracker: GitHub

Track issues and specs in `delaynore/ai-platform` on GitHub.
Use the authenticated `gh` CLI.

Before creating issues, verify the repository with:
gh repo view --json nameWithOwner

Read issues with gh issue view.
Create issues with gh issue create.
For multiline bodies, write a temporary file and use --body-file.
Use gh issue edit to update labels and assignments.
Before closing an issue, post a comment explaining its resolution.

Use GitHub sub-issues for parent-child relationships.
Use native issue dependencies for blockers.
If unavailable, record "Part of #<parent>" and "Blocked by: #<blocker>" in issue bodies.
An issue is unblocked when all blockers are closed.

When a skill requests publication to the tracker, create a GitHub issue.
When a skill requests a ticket, read its GitHub issue.

## Pull requests as a triage surface

PRs as a request surface: no.

## Wayfinding

Represent each map as an issue labelled wayfinder:map.
Link child issues as sub-issues.
Label children wayfinder:research, wayfinder:prototype,
wayfinder:grilling, or wayfinder:task.
Choose the first open child without open blockers or an assignee.
Claim it by assigning yourself.
Record the result before closing it.
Add the result and issue link to the map's Decisions-so-far.
