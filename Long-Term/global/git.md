---
scope: global
topics: [git, branches, integration, rebase, worktrees]
stability: durable
last-reviewed: 2026-07-10
---

# Git And Branch Workflow

- When a colleague branch predates an accepted refactor, create an integration branch from the accepted baseline and forward-port the feature intent. Do not rewrite the colleague branch without explicit approval.
- For follow-up MRs, identify the intended baseline first, adapt older changes to current contracts, and keep the original branch untouched.
- After a dependency branch is squash-merged or rebased, compare old and new base trees. If equal, a history-only `rebase --onto` is valid; verify the diff and use explicit `--force-with-lease` when updating remote history.
- Before mutating a copied repository, inspect `.git`, `git rev-parse --git-dir`, and `git status`. Copying a linked worktree is not isolation; use a local `--no-hardlinks` clone and set the intended remote.
