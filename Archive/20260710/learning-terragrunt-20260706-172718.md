---
type: short-term-learning
topic: terragrunt
created: 2026-07-06 17:27:18
source: codex-conversation
confidence: high
---

# Learning: Strict active external-location inputs

## Context
While reviewing `data-platform-terragrunt-catalog-short-mr/units/dbx_external_locations/terragrunt.hcl`, the user questioned `try(..., null)` around active external-location fields such as `url` and `aws_iam_role_arn`.

## Learning
For `dbx_external_locations`, active rows should fail loudly when required module input fields are missing instead of normalizing them to null. Use direct `trimspace(external_location.url)` and `trimspace(external_location.aws_iam_role_arn)` for non-destroyed rows. For optional fields, only default true omission or explicit null to null; do not hide malformed provided values. A present `file_events` block should require `mode`, while `provided_sqs_queue_url` may be omitted/null except where guard/module validation makes it required for `provided_sqs`.

## Future Use
When editing this unit, keep raw validation rows guard-friendly for aggregated `E134EXT` reporting, but keep the module input projection strict for fields that are part of the active wrapper contract.
