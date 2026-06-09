---
created: 2026-06-08T18:41:09+02:00
type: learning
domain: terraform
source: codex
---

# Learning: Publish-time module rewrites must fail on changed dependency gaps

When rewriting Terraform module sources for published packages, changed intra-repo dependencies must resolve to the newly computed version from the same pipeline. If a changed dependency is missing from `module-versions.json`, falling back to the registry's latest stable version is unsafe because it can publish a package pinned to stale dependency code.

For text-based Terraform rewrite scripts, either constrain the supported file shape very clearly or add focused tests around source/version insertion, update, comment preservation, nested module blocks, relative path resolution from the correct directory, and changed-vs-unchanged dependency version resolution.
