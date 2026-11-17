# Issue tracker: GitHub

Issues and specs live in GitHub Issues in `delaynore/ai-platform`.
Use the `gh` CLI for tracker operations.

## Conventions

- Before creating an issue, verify the target with `gh repo view --json nameWithOwner`.
- Publishing to the issue tracker means creating a GitHub issue.
- Create: `gh issue create --title "..." --body-file <file>`.
- Read: `gh issue view <number> --json number,title,body,labels,comments`.
- List: `gh issue list --state open --json number,title,body,labels`.
  Add label and state filters as needed.
- Comment: `gh issue comment <number> --body-file <file>`.
- Apply or remove labels: `gh issue edit <number> --add-label "..."` or
  `gh issue edit <number> --remove-label "..."`.
- Close: post an explanation first, then run `gh issue close <number>`.
- Use UTF-8 files and `--body-file` for multiline text.

## Issue relationships

Use GitHub's native sub-issues and blocking dependencies.
Check `gh issue create --help` and `gh issue edit --help` for supported flags.
Use `gh api` when the installed CLI lacks the required flags.

For API relationships, obtain the numeric database ID with
`gh api repos/delaynore/ai-platform/issues/<number> --jq .id`.
Use that ID where the API expects an issue ID, rather than its issue number.

If native relationships are unavailable:

- Add `Part of #<parent>` to the child body and a task-list entry to the parent.
- Add `Blocked by: #<n>, #<n>` to the blocked issue body.

An issue is unblocked when all its blockers are closed.

## Pull requests as a triage surface

**PRs as a request surface: no.**

## Wayfinding operations

The map is an issue labelled `wayfinder:map`, containing Notes,
Decisions-so-far, and Fog sections.
Link tickets to the map as sub-issues. Use `wayfinder:<type>` labels,
where type is `research`, `prototype`, `grilling`, or `task`.

Select the first open ticket in map order with no assignee and no open blockers.
Claim it with `gh issue edit <number> --add-assignee @me` before starting work.
When resolved, post the answer, close the ticket, and append a brief result
and link to the map's Decisions-so-far section.
