---
scope: global
topics: [terragrunt, generated-stacks, dependencies, guards, terratest, yaml]
stability: mixed
last-reviewed: 2026-07-10
---

# Terragrunt Preferences

## Generated Stack Evaluation

- Validate rendered `.terragrunt-stack` units in their parent context; raw `units/*` templates may depend on root and common files.
- Keep dependency-free normalization in `locals`. Read proven `dependency.*.outputs` in inputs or generated files, not in locals.
- A green HCL validation may use dependency mocks. Inspect warnings and use a no-mock or real-state-safe check when output availability matters.
- Do not scrape sibling `.terragrunt-cache` output during the same stack apply. Prefer dependency contracts, staged stacks, or caller-supplied normalized inputs.
- When Terragrunt fails after cache generation but before hooks or Terraform, a named direct-Terraform control from the rendered cache can isolate Terragrunt plumbing from generated config or provider failures.

## Generated Files And Guards

- Unsigned JSON consumed by shell or `jq` uses `disable_signature = true` with `if_exists = "overwrite"`.
- Do not use `overwrite_terragrunt` for unsigned files; ownership checks can fail with bare `EOF`. Reserve it for signed generated files.
- Lifecycle guards must fail closed, report all violations, protect final-row removal, and preserve staged `enabled -> destroy_pending -> destroyed` transitions.
- Missing, malformed, or renamed Terraform outputs are not first-apply bootstrap when state exists.
- Keep paired-guard validation ownership explicit: a small shared validator or deliberate duplicated fail-closed prelude.
- Build guard projections from named, typed locals. Do not use broad `try()` defaults that turn malformed nested YAML into empty values.
- Required active inputs should fail loudly; default only genuine omission or explicit null for optional values.

## YAML And Repository Shape

- Keep live YAML flat and readable. Use organization files for stable identity metadata, regional files for catalogs, and workspace files for bindings.
- Preserve the difference between intentionally empty and malformed or missing required data.
- Verify filename, root key, row schema, and active consumers before treating registry-looking YAML as supported.
- Keep `.gitignore` aligned with actual generated artifacts; avoid broad lock-file ignores without a deliberate repository policy.

## Terratest And Review

- Verify current upstream Terratest/Terragrunt APIs when deprecation or best practice is in question.
- Terratest `v1.0.0` uses `StackGenerateContext`, `StackRunContext`, and `StackCleanContext` and raises the Go toolchain requirement to `1.26`; verify this current-state fact before upgrading.
- Avoid `terratest/modules/test-structure` for simple stage gating; its optional dependency surface is disproportionate. Prefer a small local helper.
- For Data Platform IaC MR reviews, combine the repo-local `iac-mr-review` skill with `iac-code-style`, current code, and the relevant Jira or ADR.
- Report concrete correctness, safety, CI, migration, and test findings before broad redesign suggestions.
