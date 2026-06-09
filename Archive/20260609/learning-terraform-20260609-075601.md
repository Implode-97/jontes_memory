---
type: short-term-learning
topic: terraform
created: 2026-06-09 07:56:01
source: codex-conversation
confidence: high
---

# Learning: storage credential provider metadata review

## Context
During a read-only provider compatibility review of `data-platform-terraform-modules/modules/databricks-workspace-storage-credential/version.tf`, the user wanted a strict looks/readability/KISS/YAGNI/maintainability score without tests, Jira, or other agents.

## Learning
For the workspace storage credential leaf module, the `version.tf` shape is mostly shippable: `required_version = ">= 1.14.0"` is repo-consistent, and declaring `databricks/databricks` is necessary because child modules must declare their own provider source and version requirements. No provider block or alias belongs in the module because it uses only the default workspace-level Databricks provider. The main cleanup question is the Databricks provider constraint `>= 1.113.0, < 2.0.0`: the floor should either stay tied to a known feature/tested baseline or be documented, while the `< 2.0.0` cap is better owned by root/caller policy unless there is a known v2 incompatibility.

## Future Use
For future reviews of this file or adjacent storage/external-location modules, accept the required provider declaration and missing alias. Focus review pressure on documenting the version floor and removing or justifying reusable-module upper bounds.
