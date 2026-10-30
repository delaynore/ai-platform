# Pull request workflow

## GitHub identity

Use `dlyn-agent` for GitHub reads and contributions.
Get its token with `gh auth token --hostname github.com --user dlyn-agent`.
Keep `GH_TOKEN` process-scoped; never print or persist the token.
Before each GitHub write, verify that `gh api user --jq .login` returns `dlyn-agent`.
If agent access fails, report the missing permission and ask the owner how to proceed.

For agent commits, set command-scoped author and committer identities:

- Name: `dlyn-agent`.
- Email: `350292772+dlyn-agent@users.noreply.github.com`.

Override conflicting author and committer environment variables for the commit.
Preserve global Git identity and the default GitHub CLI account.
Use HTTPS Git with inherited credential helpers cleared and a command-scoped GitHub CLI credential helper.
Use the process-scoped agent token for Git authentication.
Never use owner SSH credentials for contributions.

Use `delaynore` for repository creation or repository/account settings only when explicitly requested.
Any other use of `delaynore` requires explicit permission for that specific action.

## Prepare review

1. Start from an updated local default branch with a clean working tree.
2. Create a separate branch for the requested change.
3. Keep independently reversible changes in separate commits.
4. Run checks appropriate to the change.
5. Push the branch and open a PR ready for review.
6. Request review from `delaynore` when opening the PR.
7. Report the PR link and verification results to the owner.

## Merge and synchronize

The owner authorizes this workflow: after approval, merge the PR, delete its branch, and update the local default branch.
Apply this authorization only to PRs created for the owner's requested work in this repository.

1. Confirm that `delaynore` approved the current PR head commit.
2. Resolve outstanding change requests and required check failures before merging.
3. Merge with a merge commit to preserve the separate commits.
4. Match the reviewed head commit when issuing the merge command.
5. Verify that GitHub reports the PR as merged.
6. Delete the PR branch on GitHub.
7. Fetch the updated default branch through the agent's HTTPS credentials.
8. Switch to the local default branch.
9. Update it with a fast-forward merge.
10. Delete the merged local PR branch.
11. Verify that the local default branch matches GitHub and the working tree is clean.

Preserve unrelated local changes and branches.
If the local branch cannot fast-forward, report the divergence instead of resetting it.
Finish by reporting the merge commit, branch deletion, and local synchronization status.
