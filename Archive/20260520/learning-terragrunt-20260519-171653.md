---
type: short-term-learning
topic: terragrunt
created: 2026-05-19 17:16:53
source: codex-conversation
confidence: high
---

# Learning: sandbox region move does not remove west-3 workspace dependency

## Context
While implementing the shared platform E2E in `data-platform-terragrunt-catalog`, the sandbox lifecycle path was moved from `eu-west-3/dbcore` to `eu-central-1/dbcore` because only `eu-central-1` has the VPC-backed workspace infrastructure needed by the sandbox flow.

## Learning
Moving only `dbx_sandbox_catalogs` and `sandbox_catalogs.yml` to `eu-central-1` is not enough to make the full root E2E green if `eu-west-3/ws_dev` still remains in the canonical root stack. The root `stack run apply` still deploys that west-3 workspace, and it can fail independently on missing regional infrastructure such as a VPC.

## Future Use
When debugging multi-region Terragrunt E2E failures, separate feature placement problems from root-topology problems. If a region lacks required shared infrastructure, either provision it explicitly or remove the dependent unit from the canonical example instead of assuming that moving a downstream feature unit is sufficient.
