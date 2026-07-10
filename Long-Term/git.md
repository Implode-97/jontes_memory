# Git And Branch Workflow

## Collaboration Branches

- When a colleague's feature branch predates a large accepted E2E or helper refactor, prefer creating a new integration branch from the accepted baseline branch and forward-porting the colleague branch's feature intent into the newer structure. Do not directly rebase, rewrite, or force-push the colleague branch unless the user explicitly asks for that.
- For follow-up MRs, first identify the intended baseline branch, then adapt older changes to current helper contracts and review style. Keep the original branch untouched so the user can review the integration branch and later MR it back into the colleague branch if needed.
- When a dependency branch has been squash-merged or rebased into the target branch, compare old and new base trees before updating an integration branch. If the trees match, `git rebase --onto origin/master <old-base>` can be a history-only move; verify the resulting diff before saying no code changed, and use an explicit `--force-with-lease` against the expected old remote head when updating the remote branch.
- Before mutating a copied repository, check whether `.git` is a directory or a linked-worktree pointer file and compare `git rev-parse --git-dir` plus `git status` in the source and copy. A literal copy of a linked worktree is not independent; for an isolated review folder, clone the local repository with `--no-hardlinks` and then set the intended remote URL.
