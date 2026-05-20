---
type: short-term-learning
topic: terragrunt
created: 2026-05-19 17:32:00
source: codex-conversation
confidence: high
---

# Learning: trim unsupported regions from the canonical example

## Context
While implementing the shared platform E2E in `data-platform-terragrunt-catalog`, the canonical example originally kept both `eu-central-1` and `eu-west-3`. The E2E could not succeed because `eu-west-3` did not have the shared VPC infrastructure needed for the workspace path.

## Learning
When a region in the Terragrunt example lacks required shared infrastructure, the correct short-term fix is to remove that region from the canonical example root and update the E2E, guard fixtures, and architecture docs to match. Moving one downstream feature, such as sandbox catalogs, is not enough if the root stack still deploys the broken regional workspace.

## Future Use
When debugging root E2E failures, check whether the failing region is actually supportable by the current shared environment. If not, trim the region from the canonical example until the missing regional prerequisites exist, then add the region back deliberately later.
