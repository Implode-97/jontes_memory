---
type: short-term-learning
topic: terraform
created: 2026-06-09 01:46:17
source: codex-conversation
confidence: high
---

# Learning: service-principal apply smoke should stay narrow

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-service-principal/tests/valid_apply_real_provider.tftest.hcl`, the user asked for a Terraform test semantics lens on `command = apply`, random naming, assertions, resource/output references, and provider setup.

## Learning
For this one-resource Databricks account service principal module, a real-provider `command = apply` smoke has meaningful value because it proves actual create/destroy behavior and provider-computed attributes such as SCIM ID, application ID, and ACL principal ID. The random `uuid()` suffix is a clean collision-avoidance fixture when it still satisfies the module naming validation. The lower-value part is asserting broad or duplicate outputs such as display-name echo, home, repos, and ACL alias equality, because that mostly freezes public output shape instead of testing provider behavior.

## Future Use
In future reviews, accept the live apply smoke with narrow cleanup: keep strong computed-ID assertions, keep the default lifecycle assertion if useful, and prefer trimming weak output-surface assertions before adding helpers or abstractions.
