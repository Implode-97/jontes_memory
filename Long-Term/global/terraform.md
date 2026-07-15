---
scope: global
topics: [terraform, modules, providers, inputs, outputs, tests, versions]
stability: durable
last-reviewed: 2026-07-15
---

# Terraform Module Preferences

## Provider Facts And Inputs

- Derive provider-owned facts such as AWS account, region, and Databricks context from configured providers, required IDs, or data sources.
- Prefer `aws_caller_identity`, `aws_region`, current-config-style Databricks data, and metastore lookups over duplicate inputs.
- Assume the commercial AWS partition `aws`; do not add China or GovCloud abstraction without a real requirement.
- Account-scoped Databricks wrappers may need an explicit deployer application ID because `databricks_current_user` requires workspace context.
- Reject ambiguous naming tokens before stateful APIs: whitespace, duplicate normalized keys, fractional IDs, and overlong generated names.

## Module Boundaries

- Name modules by their primary provider context. Use `databricks-account-*` for account Unity Catalog modules and `aws-databricks-account-*` for cross-provider wrappers.
- Keep primitive S3, IAM, and security-group modules provider-generic; place Databricks policy, pass-role, workspace, and root-storage behavior in wrappers.
- Keep contracts narrow until a real consumer requires flexibility.
- Lifecycle-sensitive registries should not silently default to empty unless empty bootstrap is an explicit contract.
- Wrapper object inputs must match child shapes or transform explicitly in a named local with focused tests.
- Before wiring a cross-repository wrapper field, verify the published typed-object input and output contract. Terraform object conversion can discard unsupported extra attributes, making an accepted-looking input a no-op.
- Put exact naming-family policy near wrappers and generators; leaf modules should enforce only the hygiene they own.
- Prefer resource-shaped generic inputs such as `inline_policy_json`; do not imply semantic IAM validation when only JSON syntax is checked.
- Use `git mv` for module renames so review history remains legible.

## Output Contracts

- Treat outputs as public API. Preserve only stable downstream contracts, automation guards, and provider-created IDs that consumers need.
- Avoid duplicate scalar and composite forms of the same facts; choose one canonical shape.
- Primitive modules should expose scalars and narrow maps, not provider-resource mirrors. Wrappers may compose a small owned contract.
- Assert internal resources, data, and locals directly in tests instead of publishing test-only outputs.
- Keep justified diagnostics narrow, labeled, and sensitive where appropriate.
- Compare variables, outputs, tests, and implementation; remove broad echoes of caller taxonomy or provider internals without consumers.

## Testing Confidence

- Prefer real provider plan or apply tests when they add practical confidence.
- Use real apply for ordinary resources when feasible; use real plan plus mock apply for expensive workspaces or broad wrappers.
- Keep default `tests/` mock-safe. Put shared-fixture or live-provider smoke tests in explicit opt-in paths or protected lanes.
- A live test must prove something mocks cannot: authentication, live lookup/API constraints, computed IDs, lifecycle, or provider rendering.
- Mock apply tests should assert resource behavior and lifecycle invariants, not mocked values echoed through outputs.
- Keep invalid-input tests compact: one valid baseline, one invalid override, precise `expect_failures`, and descriptive run names.
- Assert IAM statements exactly when safety matters: unique Sid, effect, complete actions, and complete resources.
- Ensure documented `.tfvars` and fixture paths are ignored before they can expose account IDs, ARNs, or environment data.
- Do not rely on `prevent_destroy` for test cleanup; it disappears when the resource is removed from configuration.
- Model destroy ordering deliberately. Separate workspace-binding resources can remove non-anchor access before deleting a Unity Catalog securable.

## Provider And Version Constraints

- Keep reusable provider constraints minimal and explain non-obvious patch floors with the required feature or bug fix.
- Avoid upper bounds such as `< 2.0.0` without a known incompatibility or central policy.
- Declare provider aliases only when implementation actually references them.

## Comments And Workarounds

- Document platform workarounds as `// jnyjc2 YYYY-MM-DD: practical reason.` Keep longer prose only when the code cannot explain the constraint.
