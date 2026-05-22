---
type: short-term-learning
topic: databricks
created: 2026-05-21 13:35:25
source: codex-conversation
confidence: high
---

# Learning: Databricks serverless GPU is AI Runtime, not classic GPU clusters

## Context
While researching Databricks serverless GPU public preview for future platform design, current docs showed the feature is exposed as AI Runtime on Databricks Serverless.

## Learning
Platform users select Serverless GPU / AI Runtime from notebooks or jobs, then choose an accelerator such as A10 or H100 and a managed base environment. It uses serverless environment versions rather than Databricks Runtime ML or ordinary cluster policies. The latest environment v5 release notes state Ubuntu 24.04.2, Python 3.12.3, Databricks Connect 18.0.0, and CUDA Toolkit 12.9. The v5 Standard/base GPU environment is deliberately minimal and does not include PyTorch; use AI v5 or explicit `%pip` / environment dependencies for ML libraries.

## Future Use
For platform design, treat serverless GPU as a governed, preview, region-limited capability controlled through previews, usage policies/tags, environment dependency standards, and documented notebook/job templates rather than through custom cluster runtimes, init scripts, Docker images, or compute policies.
