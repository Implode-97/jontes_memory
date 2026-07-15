---
type: short-term-learning
topic: databricks
created: 2026-07-10 15:35:29
source: codex-conversation
confidence: high
---

# Learning: Data product OIDC trust is environment-specific

## Context
This decision supersedes the draft CAV-134270 design that copied one product-level federation policy to every environment service principal. The production and non-production authentication boundaries need different subjects.

## Learning
Keep `oidc_federation` inside each data product `env_cycles` entry. Production (`prd`) policies must use the `sub` claim and select only `main` or `master`. For non-production GitLab workloads, matching the exact `project_path` claim allows tokens from any branch in that project without relying on undocumented Databricks wildcard matching. This also admits tags and merge-request tokens from the same project because a Databricks service-principal federation policy matches one subject claim exactly.

## Future Use
Do not copy federation policies across environments. Validate production subjects in Terraform and keep environment normalization in Terragrunt. Document that `project_path` is project-wide, and use GitLab CI job rules to prevent non-branch jobs from requesting the token when branch-only issuance matters.
