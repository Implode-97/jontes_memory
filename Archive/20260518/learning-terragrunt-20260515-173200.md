---
topic: terragrunt
project: data-platform-terragrunt-catalog
date: 2026-05-15
---

# Learning: guard-test CI needs jq in Terragrunt catalog

The new fast guard-test lane in `data-platform-terragrunt-catalog` runs Terragrunt-generated hook scripts that invoke `jq`. Installing only Go in the `alpine/terragrunt` CI image is not enough.

When updating or recreating the guard-test CI job, include `jq` in `apk add --no-cache` or the guard suites will fail early with messages like `jq is required for sandbox catalog input guard` and `jq is required for team identity destroy guard`.
