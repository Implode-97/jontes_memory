---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:58:13
source: codex-conversation
confidence: high
---

# Learning: Documented local tfvars files must be ignored

## Context
During the module scoring run, an IAM module README told users to copy `tests/real.auto.tfvars.example` to `tests/real.auto.tfvars`, but the repo ignore rules only ignored a different module's real tfvars path.

## Learning
When README or test docs instruct users to create local `.tfvars`, `.auto.tfvars`, or other real-provider fixture files, the exact documented path must be covered by `.gitignore` or use a clearly ignored/local-only filename pattern. This matters especially for real smoke tests because fixture files can contain account IDs, ARNs, temporary test names, or environment-specific values that should not be committed accidentally.

## Future Use
When reviewing Terraform test documentation, check the documented local fixture paths against `.gitignore`. Penalize docs that tell users to create unignored real-provider files, and prefer either opt-in real test directories with ignored fixture names or CI-provided variables.
