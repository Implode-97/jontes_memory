---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:01:17
source: codex-conversation
confidence: high
---

# Learning: sandbox wrapper provider constraint score

## Context
During the sandbox catalog wrapper scoring series, `aws-databricks-workspace-catalog-wrapper-sandbox/version.tf` was reviewed for provider semantics, readability, KISS, YAGNI, and maintainability.

## Learning
The provider requirements file is high quality and shippable, scoring in the high 80s. Its explicit `required_version = ">= 1.14.0"` matches sibling module style. Declaring AWS, Databricks, and Time providers is justified because the wrapper directly uses AWS data sources, a Databricks account-scoped data source, downstream workspace Databricks modules, and `time_sleep`. `configuration_aliases = [databricks.account]` is required and correct because callers must pass the account provider explicitly.

## Future Use
For future reviews, accept the provider declarations as structurally correct. The narrow cleanup question is whether Databricks `< 2.0.0` is a documented compatibility boundary or unnecessary upper-bound friction for a reusable module; if no known incompatibility exists, prefer a minimum-only constraint or document the major-version cap.
