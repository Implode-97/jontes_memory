# CI Preferences

## Data Platform Terragrunt Catalog

- In `data-platform-terragrunt-catalog`, Codex MR review must stay asynchronous relative to `fmt`, `guard-tests`, and `platform-e2e-test`. A small detect-stage gate such as `print-changed-units` is acceptable, but Codex review should not block the real test lane.
- The shared-environment E2E lane is the authoritative serialized CI lane for real deploy validation.
- The fast guard-test lane uses Terragrunt-generated hook scripts that call `jq`. In the `alpine/terragrunt` CI image, install `jq` with `apk add --no-cache` whenever creating or recreating the guard-test job, or the guard suites fail before useful assertions run.
