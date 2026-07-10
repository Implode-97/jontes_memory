---
type: short-term-learning
topic: terragrunt
created: 2026-07-03 16:51:58
source: codex-conversation
confidence: high
---

# Learning: Direct Terraform Cache Probe For Terragrunt EOFs

## Context
During investigation of sandbox_catalog E2E failures, direct Terragrunt no-op applies for generated Databricks units returned EOF before clear Terraform/module execution. The user wanted a probe that could distinguish Terragrunt execution-layer failures from bad generated Terraform inputs/provider/backend state.

## Learning
When Terragrunt appears to fail before entering a generated unit, a useful control is to run Terraform directly from Terragrunt's generated `.terragrunt-cache` working directory using `terragrunt-debug.tfvars.json`. Print generated files first, including `terragrunt.hcl`, `terragrunt.values.hcl`, debug tfvars, generated backend/provider files, and guard JSON. Then run `terraform init` and `terraform apply -var-file=<debug tfvars>` from the cache.

## Future Use
For similar CI EOF investigations, treat direct Terraform success as evidence that module inputs, provider config, backend config, and state are healthy, narrowing suspicion to Terragrunt dependency/generation/execution behavior. Treat direct Terraform failure as a sign that Terragrunt may be masking a lower-level generated config, backend, provider, state, or module issue.
