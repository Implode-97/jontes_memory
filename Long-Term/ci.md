# CI Preferences

## Terraform Module Repositories

- In older Terraform module CI, prefer detecting changed modules and generating targeted dynamic jobs for those modules plus modules that depend on them, instead of running every module on every change.
- For non-master Terraform module publishing, preserve SemVer-aware release candidates when that repository still uses this pattern: the first non-master publish can start at `0.0.0-rc.1`, and unclear branch types can fall back to a `bugfix` bump.
- Check current repository state before applying these historical CI preferences; they came from earlier module-repository work and may have evolved.

## Data Platform Terragrunt Catalog

- In `data-platform-terragrunt-catalog`, Codex MR review must stay asynchronous relative to `fmt`, `guard-tests`, and `platform-e2e-test`. A small detect-stage gate such as `print-changed-units` is acceptable, but Codex review should not block the real test lane.
- The shared-environment E2E lane is the authoritative serialized CI lane for real deploy validation.
- The fast guard-test lane uses Terragrunt-generated hook scripts that call `jq`. In the `alpine/terragrunt` CI image, install `jq` with `apk add --no-cache` whenever creating or recreating the guard-test job, or the guard suites fail before useful assertions run.
