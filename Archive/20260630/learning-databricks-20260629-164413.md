---
type: short-term-learning
topic: databricks
created: 2026-06-29 16:44:13
source: codex-conversation
confidence: high
---

# Learning: Keep Databricks FUSE Diagrams Runtime-Focused

## Context
The user corrected a Databricks Unity Catalog volumes/FUSE diagram because it included local repo and platform implementation context. That made the drawing less useful for understanding Databricks runtime communication channels.

## Learning
For Databricks UC volumes, FUSE, Spark, and auth explanations, avoid baking in Terraform, Terragrunt, sandbox catalog, wrapper modules, or current implementation details unless explicitly requested. The user wants conceptual diagrams centered on communication paths: driver-local native OS reads, FUSE/POSIX `/Volumes` access, Spark datasource reads through driver and executors, direct cloud URI access, UC auth/policy checks, and cloud storage credential boundaries.

## Future Use
When asked for Databricks architecture diagrams, first separate supported runtime access channels from local implementation concerns. Label inferred Databricks internals as conceptual abstractions and keep caveats about executor-native paths, RDDs, UDFs, access modes, and runtime versions explicit but visually secondary.
