# CI Preferences

## Terraform Module Repositories

- In older Terraform module CI, prefer detecting changed modules and generating targeted dynamic jobs for those modules plus modules that depend on them, instead of running every module on every change.
- In `data-platform-terraform-modules`, the dynamic module test runner only executes `tests/*.tftest.hcl`; wrapper `real_tests/` are not covered by ordinary module CI unless they are wired into a separate job. Use protected, manual, or scheduled CI for live wrapper apply smoke tests that need shared Databricks resources.
- For non-master Terraform module publishing, preserve SemVer-aware release candidates when that repository still uses this pattern: the first non-master publish can start at `0.0.0-rc.1`, and unclear branch types can fall back to a `bugfix` bump.
- In `data-platform-terraform-modules`, publish-time source rewriting in `ci/scripts/deploy-changed-modules/rewrite_publish_module_sources.py` should resolve unchanged local dependency modules by exact Terraform registry identity `<module>/<system>`, not by bare module name. GitLab package lookup by `package_name=<module>` can return similarly prefixed wrapper packages and generate child module version pins that do not exist.
- When republishing Terraform wrappers after dependency lookup fixes, README-only touches to wrappers and direct child modules can be intentional version triggers rather than Terraform behavior changes. Preserve those touches unless the release strategy changes so wrappers can pin freshly published child module versions.
- Check current repository state before applying these historical CI preferences; they came from earlier module-repository work and may have evolved.

## Data Platform Terragrunt Catalog

- In `data-platform-terragrunt-catalog`, Codex MR review must stay asynchronous relative to `fmt`, `guard-tests`, and `platform-e2e-test`. A small detect-stage gate such as `print-changed-units` is acceptable, but Codex review should not block the real test lane.
- The shared-environment E2E lane is the authoritative serialized CI lane for real deploy validation.
- The fast guard-test lane uses Terragrunt-generated hook scripts that call `jq`. In the `alpine/terragrunt` CI image, install `jq` with `apk add --no-cache` whenever creating or recreating the guard-test job, or the guard suites fail before useful assertions run.
- For Terratest and Terragrunt failure investigations, fetch the real GitLab job trace when it is accessible. Pasted log excerpts are fallback evidence only.
- Save the raw trace and run the repo-local `skills/ci-log-triage/scripts/filter_ci_log.py` before diagnosing. Start from the first retained failure block; later cleanup errors are often fallout from the earliest contract, permission, or apply failure.
- For `platform-e2e-test` failures during Terraform init in generated `dbx_metastore` or `dbx_workspace` units, check GitLab Terraform module registry publication state and generated wrapper package tarballs before debugging Databricks auth or cleanup fallout. Missing or wrong child module versions can surface as `Unresolvable module version constraint`.
- For catalog MR !21 / CAV-132831 E2E retries, keep separating first apply failure from cleanup fallout. If `dbx_metastore` fails creating Databricks account group `platform-tratondev-dmp-euc1-metastore-admins` because it already exists, clean up or import that orphaned group before rerunning; janitor destroys may still look incomplete when workspace or metastore dependency outputs are unavailable during destroy.
- Sandbox catalog external-location failures that appear immediately after IAM role policy or trust creation can be AWS/Databricks propagation rather than missing Terraform inputs. When the plan has the expected `credential_name` and IAM policy shape, prefer a readiness wait or retry before Databricks external-location validation.
