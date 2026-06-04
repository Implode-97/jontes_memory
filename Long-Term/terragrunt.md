# Terragrunt And IaC Memory

## Data Platform Terragrunt Catalog

- For `data-platform-terragrunt-catalog`, prefer one shared real E2E lane in CI instead of multiple per-stack real deploy jobs.
- Root test flow should own mandatory setup and cleanup: copy the example tree, generate stacks, run root apply, defer destroy, and clean stacks.
- Preserve feature-based E2E stages, with names such as `team_identities_assert`, `workspace_assert`, and `catalog_sandbox_assert`, but avoid importing broad Terratest packages when only simple opt-in/out stage gating is needed.
- For the current CAV-132831 shared E2E scope, `wavebreakers` is the canonical team name and derived values should align to it, including `cc-wavebreakers`. Keep workspace assignment units/jobs, account budget policy units, old stack tests/jobs, and live mock locals out unless the user explicitly expands scope; the migration target is the new shared `platform-e2e-test`.
- Treat local real apply as optional debug work, not the promised default V1 workflow. Keep fast guard checks outside the real deploy lane and require explicit CI mutual exclusion for shared real deploys.
- Cleanup reliability is a first-order design concern for shared Databricks Terragrunt E2E tests. Try root `terragrunt stack run destroy` first, then fall back to reverse-order generated-unit destroys when the root graph is broken. Keep Terraform-owned janitors as the primary cleanup mechanism, and reserve direct API force cleanup for protected/manual recovery paths. Preserve the copied `example/non-prod-env-e2e` tree when cleanup fails so local state and generated unit files remain available for manual reruns.
- Before rerunning shared catalog E2E after a failed cleanup, preflight leaked `sandbox_*` Unity Catalog resources such as catalogs, storage credentials, and external locations. The CI example has commented remote state, so local Terraform state can disappear with the job workspace while stable names such as `sandbox_firecrackers` remain; clean leaks with a principal that can manage the metastore or catalog before treating the next run as a code retry.
- Add explicit cleanup targets for new E2E features, and add fast guard or pre-hook tests before real apply. Guard coverage should catch lifecycle hook asymmetry between `plan`/`apply` and `destroy`, dependency outputs needed during destroy-time provider rendering, mock allow-lists that hide real destroy inputs, and output-driven prior-state row removals.
- Terraform output assertions are useful, but higher confidence comes from Databricks API checks that resources really exist and were removed.
- Keep the canonical example root aligned with currently supported regional infrastructure. Moving a downstream feature unit, such as sandbox catalogs, to a supported region is not enough if the root stack still deploys an unsupported regional workspace; either provision the missing shared prerequisites or trim the unsupported region until it can be added back deliberately.
- In `data-platform-terragrunt-catalog`, centralize the Databricks deployer service-principal application/client ID in `example/non-prod-env/_common_vars.hcl` as `databricks_deployer_service_principal_application_id = get_env("DATABRICKS_CLIENT_ID")`. Feature units such as `dbx_metastore` and `dbx_workspace` should pass `local.common.databricks_deployer_service_principal_application_id` to wrapper inputs.
- When validating generated stack units, inspect the rendered `.terragrunt-stack` units under `example/non-prod-env/...`; raw `units/*` templates can fail outside their parent root/common file context.
- For catalog MR !21 / CAV-132831 continuation, the corrected registry pins from modules pipeline `7662011` are `databricks-account-metastore-wrapper/databricks` `0.4.0`, `databricks-account-metastore/databricks` `0.4.0`, `aws-databricks-workspace-wrapper/aws` `0.5.0`, and `aws-databricks-workspace/aws` `0.5.0`; branch commit `fe3f660` updated `dbx_metastore` and `dbx_workspace` to those wrapper versions.
- The `dbx_sandbox_catalogs` apply-only `before_hook` controlled by `DBX_SANDBOX_PREREQ_WAIT_SECONDS`, defaulting to 900 seconds, is a diagnostic and stabilization probe for fresh workspace/metastore propagation. If it stabilizes E2E, replace or narrow it with a readiness check at the Terragrunt boundary before workspace-level UC consumers; do not turn the fixed sleep into permanent architecture or expand sandbox lifecycle coverage until the baseline path is stable.
- After the sandbox catalog wrapper package rename, catalog units should use `aws-databricks-account-catalog-wrapper-sandbox/aws`, not the old `aws-databricks-catalog-wrapper-sandbox/aws`. Renamed GitLab Terraform module packages can reset to version `0.1.0`, so verify the exact package name and version in the registry instead of assuming monotonic versions across renames.

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
