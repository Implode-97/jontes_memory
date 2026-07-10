---
scope: repo
repo: data-platform-terragrunt-catalog
topics: [terragrunt, catalog, platform-e2e, guards, generated-stack, databricks, data-products]
stability: mixed
last-reviewed: 2026-07-10
---

# data-platform-terragrunt-catalog

## Core Repository Contract

- Use one serialized shared-environment E2E lane rather than separate real-deploy jobs per stack.
- Root test flow owns copied-environment setup, stack generation, apply, feature assertions, deferred destroy, and stack cleanup.
- Keep local real apply as optional debugging, not the default V1 workflow.
- Keep feature assertions as one top-level stage each; split private helpers without adding independent stage state unless rerun/skip behavior needs it.
- Databricks API existence and removal checks provide stronger confidence than Terraform outputs alone.
- Centralize the deployer application ID in `_common_vars.hcl` from `DATABRICKS_CLIENT_ID` and pass it to metastore/workspace wrappers.
- Treat TOOCAN as a product/platform-owner decision gate until Terraform/Terragrunt control equivalence is accepted.

## CI And Tooling

- Keep Codex review asynchronous and non-authoritative relative to format, guard, and platform E2E jobs.
- Pin Terragrunt through `TERRAGRUNT_VERSION` and the checksum-verified installer; the Alpine image tag identifies Terraform more reliably than Terragrunt.
- Guard-test jobs must install `jq` before running generated shell hooks.
- CI module sources must use registry packages unless the caller explicitly supplies a local source override.
- Give OIDC/STS credentials enough lifetime for the test and deferred cleanup; parse credentials structurally and preflight them.
- Before rerunning a failed shared environment, inspect the first failure, cleanup result, and leaked AWS, Databricks, or Terragrunt resources.
- If cleanup reports `BucketNotEmpty` on workspace root storage, empty the E2E bucket or use protected recovery; do not add implicit `force_destroy` to the reusable secure-bucket primitive.

## State, Apply, And Cleanup

- E2E uses active S3 remote state with `use_lockfile = true`, generated backend metadata, tagged bucket creation, and keys under `data-platform-terragrunt-catalog/non-prod-env`.
- Do not assume local state or DynamoDB locking. Terraform janitors need initialized cache directories with generated backend metadata.
- On Terragrunt v1.1, use `terragrunt stack run --backend-bootstrap apply`; do not place the flag after `--`.
- Never pre-bootstrap all generated units when downstream provider/input generation requires dependency outputs.
- Try root stack destroy first, then reverse-order generated-unit Terraform janitors. Preserve the copied tree when cleanup fails.
- Keep API force deletion in a separate protected/manual recovery path with deterministic test-resource filters and dry-run output.
- A later S3 `NoSuchBucket` may mask the original `CreateBucket` failure. Inspect CloudTrail, IAM, SCP, and request tags before changing command syntax.
- Creation-time bucket tags require `s3:TagResource` in addition to `s3:CreateBucket` and `s3:PutBucketTagging`.

## E2E And Guard Design

- Keep shared E2E as a full-stack converge/destroy smoke plus feature output assertions.
- Put lifecycle transitions and unsafe-delete matrices in fast guard tests unless repeated live applies are an explicit cost/stability probe.
- Add cleanup targets and guard coverage with every new feature, including destroy-time dependencies, mock allow-lists, and prior-state row removal.
- Guard tests should execute the real Terragrunt and shell-hook boundary with narrow fake `terraform output -json` fixtures.
- Keep the main E2E path clear; broad diagnostics and cache scanning belong in named recovery helpers.
- Classify external-location failures before editing code: assume-role, S3 read/list, role ARN or external ID, file events, workspace/metastore readiness, or wrong generated policy inputs.
- A green wrapper real test does not prove fresh-workspace catalog E2E; shared fixtures and randomized names exercise a different topology.
- Decode output JSON through a `map[string]json.RawMessage` envelope and typed helpers for asserted keys; avoid broad mirror structs and fully dynamic assertions.
- Each stage owns its feature contract. Downstream stages may read upstream outputs as fixtures but should not re-test broad upstream bundles.

## Sandbox Catalogs And External Locations

- Preserve sandbox lifecycle guard JSON, tombstones, and order-only team-identity dependencies.
- Prefer named locals and explicit hooks over dense inline JSON construction or surprising init cleanup.
- Fresh workspace/metastore readiness may lag Terraform-visible assignment. Treat fixed waits as evidence and move toward bounded polling.
- `dbx_external_locations` active rows require non-empty `url` and `aws_iam_role_arn`; guard projections may aggregate validation, but module inputs fail loudly.
- Unsigned guard JSON follows `global/terragrunt.md`: plain `overwrite`, never `overwrite_terragrunt`.
- Source-owned external-location access and file-event semantics live in `domains/databricks-unity-catalog.md`.

## Compute Policies

- Derive default team eligibility from active `workspace_teams.yml` rows and enabled `_org/teams.yml` rows.
- Use `grp_team_<team_key>_contributor` as the default principal and keep workspace assignment as ordering-only unless a proven output contract is required.
- Store profiles in `_org/cluster_policy_profiles.yml` under `compute_profiles`; profiles use `definition_file`.
- `is_default: true` marks automatic low-risk profiles; explicit team requests use `compute_profile_ids`.
- Propagate team `cost_center` and `unit_name` into authoritative custom tags.
- Keep product execution policies separate until a current implemented consumer requires them.

## Data Product Catalogs

- Keep `dbx_data_product_catalogs` separate from sandbox catalogs with its own wrapper and `data_products.yml` registry.
- Each row has an explicit `env_cycles` map and copies provider-neutral OIDC federation into each environment principal.
- Each active environment creates one service principal, federation policies, bucket, external location, and catalog.
- Lifecycle states are `enabled`, `destroy_pending`, and `destroyed`; row removal is blocked until destroyed apply.
- `break_glass_enabled` is per environment, defaults true except for production, and grants the owner-team group.
- Name workspace dependencies by role, such as `provider_workspace`; reserve `dev_workspace` for genuinely dev-only behavior.

## Review Heuristics

- Accept thin selectors and explicit one-unit stacks when folder, target, dependency path, active unit, and colocated YAML agree.
- Prove provider-parent consumers before judging line count. Remove dormant alternate modes and duplicate generated-provider ownership.
- Cross-workspace selectors must expose the generated dependency path; `workspace_target_name` alone is not proof.
- Remove orphan roots, dormant backend blocks, unused locals, and stale DynamoDB commentary.
- The active `dbx_workspace` unit should keep live wiring in Terragrunt and detailed AWS/Databricks behavior in its wrapper.

## Current State — Verify

- Terragrunt v1.1 behavior, the deployed S3 Tag/Untag permission fix, registry package names, and wrapper versions must be verified before changing them.
- The sandbox wrapper package family uses `aws-databricks-account-catalog-wrapper-sandbox/aws`; renamed packages may restart at `0.1.0`.
- Fixed readiness waits, exact example teams, prior MR boundaries, pipeline IDs, and branch commits are historical evidence in `Dreams`/`Archive`, not durable contracts.
