---
type: short-term-learning
topic: terraform
created: 2026-06-09 04:37:21
source: codex-conversation
confidence: high
---

# Learning: Team identities wrapper version constraint score

## Context
During a read-only multi-pass scoring review of `data-platform-terraform-modules/modules/databricks-account-team-identities-wrapper/version.tf`, the user wanted a strict KISS/YAGNI Terraform version-constraint judgement.

## Learning
The file is shippable and scores in the low-to-mid 80s. Its `required_version = ">= 1.14.0"` is simple and matches sibling module style. Declaring the Databricks provider is acceptable because the wrapper composes account group and group-membership child modules, although it is partly duplicate contract surface because the wrapper has no direct Databricks resources or data sources. The main maintainability/KISS penalty is the reusable-module provider cap `version = ">= 1.110, < 2.0"` when no known Databricks v2 incompatibility or repo policy is stated.

## Future Use
For future reviews, accept this file with narrow cleanup. Prefer dropping `< 2.0` or documenting a central repo policy for the Databricks major-version cap. Do not treat the file as needing rework or blocking.
