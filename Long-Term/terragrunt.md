# Terragrunt And IaC Memory

## Data Platform Terragrunt Catalog

- For `data-platform-terragrunt-catalog`, prefer one shared real E2E lane in CI instead of multiple per-stack real deploy jobs.
- Root test flow should own mandatory setup and cleanup: copy the example tree, generate stacks, run root apply, defer destroy, and clean stacks.
- Preserve feature-based E2E stages, with names such as `team_identities_assert`, `workspace_assert`, and `catalog_sandbox_assert`, but avoid importing broad Terratest packages when only simple opt-in/out stage gating is needed.
- Treat local real apply as optional debug work, not the promised default V1 workflow. Keep fast guard checks outside the real deploy lane and require explicit CI mutual exclusion for shared real deploys.
- Cleanup reliability is a first-order design concern for shared Databricks Terragrunt E2E tests. Add fast guard or pre-hook tests before real apply, make destroy behavior observable, and consider fallback cleanup for account-level resources that are easy to strand.
- Terraform output assertions are useful, but higher confidence comes from Databricks API checks that resources really exist and were removed.
- Keep the canonical example root aligned with currently supported regional infrastructure. Moving a downstream feature unit, such as sandbox catalogs, to a supported region is not enough if the root stack still deploys an unsupported regional workspace; either provision the missing shared prerequisites or trim the unsupported region until it can be added back deliberately.

## Live YAML Inputs

- Live Terragrunt YAML files should describe platform intent in flat, readable structures rather than deeply nested documents.
- Use `_org/teams.yml` for stable team metadata, optional role override files only for deviations, region or metastore scoped files for sandbox and data product catalogs, and workspace scoped files for bindings such as policies.
- Preserve the difference between "intentionally empty" and "malformed or missing." Do not silently normalize a missing required top-level key to an empty list.

## Terratest Usage

- For Terratest Terragrunt work, verify against current upstream source when deprecations or best-practice drift are in question instead of relying on older docs.
- Terratest `v1.0.0` uses `StackGenerateContext`, `StackRunContext`, and `StackCleanContext`; upgrading to it raises the Go toolchain requirement to `1.26`, which must be reflected in CI before the upgrade is complete.
- Avoid `github.com/gruntwork-io/terratest/modules/test-structure` for simple stage gating in this repo. In Terratest `v1.0.0`, it compiles save/load helpers that import heavy optional dependencies such as AWS, Kubernetes, Packer, SSH, OPA, and Terraform. Prefer a tiny local helper unless those save/load helpers are genuinely needed.

## Serverless Budget Policy Reviews

- The serverless budget policy feature moved from workspace-scoped to account-wide/account-scoped. Do not treat account scope or empty workspace bindings as automatically wrong.
- Still flag stale Jira, docs, or ADR text if it describes a workspace-scoped contract after the implementation has moved to account-level Databricks serverless usage policy semantics.
- For `dbx_budget_policy` or successor `serverless-budget-policy` work, review account-level naming collision risk, cost attribution tags, and explicit E2E or CI rollout decisions.

## Review Workflow

- The `ng-iac` workspace contains a repo-local `skills/iac-mr-review` skill for reviewing colleague MRs in Data Platform IaC repos.
- For future catalog, module, and live MR reviews, combine `iac-mr-review` with `iac-code-style`, fetch or inspect the MR branch, and read the related Jira story, Need field, or ADR before accepting scope changes.
- Prioritize concrete correctness, safety, CI, migration, and test gaps. Report findings first with severity and file references, not broad rewrites of the MR.
