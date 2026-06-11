# Git And Branch Workflow

## Collaboration Branches

- When a colleague's feature branch predates a large accepted E2E or helper refactor, prefer creating a new integration branch from the accepted baseline branch and forward-porting the colleague branch's feature intent into the newer structure. Do not directly rebase, rewrite, or force-push the colleague branch unless the user explicitly asks for that.
- For follow-up MRs, first identify the intended baseline branch, then adapt older changes to current helper contracts and review style. Keep the original branch untouched so the user can review the integration branch and later MR it back into the colleague branch if needed.
