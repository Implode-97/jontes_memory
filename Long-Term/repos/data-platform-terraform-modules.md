---
scope: repo
repo: data-platform-terraform-modules
topics: [terraform-modules, publishing, registry, tests, wrappers, real-provider]
stability: mixed
last-reviewed: 2026-07-10
---

# data-platform-terraform-modules

## CI And Publishing

- Prefer targeted dynamic jobs for changed modules and their dependents when the current runner supports that model.
- The dynamic runner executes plain `terraform test` for selected modules, so every `tests/*.tftest.hcl` file enters normal MR CI.
- Keep live shared-resource tests out of default `tests/`; use `real_tests/` plus an explicit protected, manual, or scheduled lane.
- Non-default publishing may use SemVer release candidates, starting at `0.0.0-rc.1`; verify the repository's current bump rules before applying historical defaults.
- `ci/scripts/deploy-changed-modules/rewrite_publish_module_sources.py` resolves unchanged local dependencies by exact registry identity `<module>/<system>`, not bare module name.
- Fail publishing when a changed intra-repo dependency is missing from the same pipeline's computed versions.
- README-only touches to wrappers or direct children may intentionally trigger republishing after dependency-lookup fixes; preserve them unless release strategy changes.
- Wrappers requiring caller-supplied aliases such as `databricks.account` may fail standalone `terraform validate`. Keep provider-aware tests and skip only the unsuitable generic validation loop.

## Module And Test Contracts

- Apply `global/terraform.md` for module boundaries, public outputs, provider facts, and test confidence.
- Apply `domains/databricks-governance.md` and `domains/databricks-unity-catalog.md` for Databricks semantics; do not restate those contracts in wrapper-specific outputs.
- Real-provider tests use pinned IDs for actual shared fixtures, never all-zero placeholders.

## Current State — Verify

- Shared account-level test deployer application ID: `f425e414-e206-48d5-80ab-6d537930d0d7`.
- Sandbox wrapper fixture uses host `https://dbc-b72ad987-f679.cloud.databricks.com`, workspace ID `4022911453310458`, and region `eu-west-1`.
- Its account ID is `47dc11d5-0e29-4a29-b9b0-9d3d3961e518` and metastore ID is `3f86dad4-0a1b-4efb-9bcd-e32a1a541300`.
- Verify these shared IDs, package names, package versions, and active CI lanes against the current repository and registry before use.
