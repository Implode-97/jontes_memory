---
type: short-term-learning
topic: databricks
created: 2026-06-08 22:38:22
source: codex-conversation
confidence: high
---

# Learning: Document Lookup-Module Cross-Input Contracts

## Context
While scoring Databricks account group membership module files, several reviewers found that the code shape was simple but some behavior was only obvious after reading adjacent files.

## Learning
For lookup-only Terraform modules, especially Databricks SCIM/account identity helpers, validation should focus on lookup hygiene and real safety invariants. Cross-input behavior such as “at least one member set must be non-empty” or “the target group cannot be nested into itself” can live in resource/data-source preconditions, but the public variable descriptions and README should still mention it. Avoid making Terraform own undocumented provider/API limits, such as copied max-length checks, unless the source is known and the reason is documented.

## Future Use
When reviewing or writing Databricks lookup modules, keep typed inputs and simple validation, but check whether public docs describe cross-input contracts and whether any hard numeric provider/API limits are sourced or removable.
