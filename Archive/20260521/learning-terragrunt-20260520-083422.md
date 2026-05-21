---
type: short-term-learning
topic: terragrunt
created: 2026-05-20 08:34:22
source: codex-conversation
confidence: high
---

# Learning: E2E cleanup should preserve state and fall back to unit janitors

## Context
This came from debugging the Terragrunt catalog platform E2E after partial apply failures left behind Databricks groups and metastores. The user wants CI runs to start clean and end clean, but does not want the harness to depend on state surviving across separate GitLab jobs.

## Learning
For this repo, the minimum reliable cleanup pattern is: try root `terragrunt stack run destroy`, then fall back to reverse-order generated-unit destroys when the root graph is already broken. If cleanup still fails, preserve the copied `example/non-prod-env-e2e` tree so local state and generated unit files remain available for manual destroy reruns. Destroy-time dependency mocks may need to be explicitly allowed for units such as `dbx_sandbox_catalogs`.

## Future Use
When extending the platform E2E, add new feature cleanup targets explicitly rather than assuming the root stack destroy is enough. Prefer small, stage-aligned janitor entries per generated unit, and avoid deleting the copied E2E tree when cleanup errors occur. Treat broad cross-environment preflight scanners as a later enhancement, not the first cleanup hardening step.
