# Terraform Module Preferences

## Provider Facts And Inputs

- Avoid passing account, region, Databricks account, and similar provider-owned facts as ordinary inputs when they can be reliably derived from configured providers, required IDs, or data sources.
- Prefer `aws_caller_identity`, `aws_region`, Databricks current-config style data sources, and metastore lookups when those values are part of provider context.
- Hard-code the normal commercial AWS partition as `aws`; do not add `aws_partition` inputs or `data.aws_partition` just to support China or GovCloud unless the user explicitly changes that enterprise assumption.
- Databricks deployer identity is not always provider-derivable. `databricks_current_user` is a workspace-level data source, so account-scoped wrappers should accept an explicit `deployer_service_principal_application_id` from Terragrunt or CI config and resolve it through account-compatible service-principal data sources. Validate provider scope before relying on current-user lookup.
- For reusable modules, reject ambiguous canonical naming tokens rather than silently trimming or deduplicating them when those tokens generate stable Databricks, AWS, or platform identity names. Check raw whitespace, duplicate normalized keys, fractional numeric IDs, and generated-name length before stateful APIs see them.

## Module Boundaries

- Name module folders by their primary provider context rather than only by domain. Unity Catalog account-scoped modules should use `databricks-account-*`; wrappers combining AWS resources with Databricks account-scoped behavior should use `aws-databricks-account-*`. Use `git mv` for renames so MRs show clean moves.
- Keep lower-level Terraform modules such as secure S3 bucket, IAM role, and security group modules generic enough for non-Databricks use.
- Put Databricks-specific policy documents, descriptions, pass-role defaults, workspace wiring, and root-storage semantics in Databricks wrapper modules or Terragrunt composition.
- If a reusable AWS module contains hard-coded Databricks language, move that specificity into the wrapper unless the module contract is truly Databricks-specific.
- Keep module contracts narrow until real requirements appear. Do not add fake flexibility for arbitrary principals, regions, accounts, or ownership variants before there is a concrete use case; an explicit deployer service-principal application ID is a scoped CI identity contract, not a license to add generic principal knobs.
- For lifecycle-sensitive registry inputs, avoid silent `default = []` unless an empty registry is a deliberate bootstrap path. Missing rows can affect tombstones, destruction, and Terragrunt removal guards, so prefer required registries or explicit empty lists from callers.
- Wrapper pass-through inputs should match the child module object shape exactly or transform explicitly in a named local with focused tests. Parallel wrapper types with renamed or unused attributes can silently lose caller intent through Terraform object conversion.
- Put exact naming-family validation near the wrappers or generators that create names. Leaf lookup modules should keep hygiene checks simple unless they truly own platform naming policy.
- Keep lower-level AWS module inputs generic and resource-shaped. For example, prefer `inline_policy_json` or `permissions_policy_json` over consumer-specific names like `crossaccount_policy_json`, and describe syntax-only JSON checks as syntax checks, not IAM semantic validation.

## Output Contracts

- Treat Terraform outputs as public API. Preserve narrow downstream contracts, automation guards, and provider-created IDs that callers consume; push test-only diagnostics, provider internals, and whole child-module passthroughs out before the module API freezes.
- Avoid publishing duplicate scalar and composite shapes for the same facts unless a real consumer needs both. Prefer one canonical output shape for grants, workspace bindings, storage credentials, catalogs, policies, and wrapper AWS/MWS identifiers.
- Tests should not freeze broad output mirrors. If a value is only used by the module's own `.tftest.hcl`, prefer direct resource, data-source, or local assertions instead of adding a public output.
- Diagnostic outputs can be justified for real bootstrap troubleshooting, such as Databricks metastore readiness, workspace URL/ID materialization, or generated policy JSON, but label or minimize them and keep sensitive values marked sensitive.
- For caller-prepared object modules, compare `variables.tf`, `outputs.tf`, and tests against `main.tf`. Validate and expose resource-owned behavior first; treat broad echoes of caller taxonomy, tags, raw principals, or rendered JSON as cleanup candidates unless downstream automation consumes them.

## Testing Confidence

- Prefer real provider plan or apply tests where they give meaningful confidence and are practical to run.
- For ordinary resource modules, real apply tests are preferred when credentials and environment allow it. For expensive or slow resources such as full Databricks workspaces or broad wrappers, use real provider plan tests and mock-provider apply tests.
- Do not rely on `prevent_destroy` as a cleanup safety net in tests; if a resource is removed from code, Terraform no longer enforces that lifecycle rule.
- Focus Terraform tests on contract, naming, policy shape, invalid inputs, and lifecycle-sensitive behavior. State clearly when validation is limited to plan or mocks.
- Keep default `tests/` mock-safe unless ordinary MR CI is intentionally expected to touch live providers. Real-provider plan or apply smoke tests that need shared AWS or Databricks fixtures belong in opt-in paths such as `real_tests/`, protected/manual lanes, or explicit filtered CI.
- Real-provider tests should prove live-only value: provider authentication, live lookup/API constraints, provider-computed IDs, lifecycle create/destroy behavior, or policy rendering that mocks cannot prove. Hybrid real/mocked tests with fake IDs score poorly unless the remaining live part matters.
- Mock apply tests should assert provider-safe resource behavior and lifecycle invariants directly: open versus isolated mode, explicit workspace bindings, account group membership edges, permission assignments, or generated IAM policy statements. Trim assertions that only read back mocked values through broad outputs.
- Invalid-input mock plan tests should stay compact: one valid baseline, one invalid override per `run`, precise `expect_failures`, and run names that distinguish variable validation from resource preconditions. Avoid null/type matrices and Databricks privilege/provider existence checks unless the module owns those semantics.
- For IAM policy documents, prefer exact normalized assertions for expected statements: Sid uniqueness, effect, complete action sets, and complete resource sets. Use subset checks only for low-risk smoke tests and make messages match what is actually asserted.
- When README or tests tell users to create local `.tfvars` or `.auto.tfvars` fixture files, verify the exact documented path is ignored. Real-provider fixture files can contain account IDs, ARNs, names, or environment values that should not be committed.
- For dependent child resources, Terraform destroys children before parents. Databricks Unity Catalog modules intentionally use separate explicit workspace-binding resources so non-anchor workspace access is removed before the securable is deleted.
- For Databricks external locations, do not treat `enable_file_events` as a standalone boolean. Terraform requires queue configuration such as `file_event_queue` plus the matching IAM, tests, and rollout notes; keep file events outside the default module contract until that complete feature is implemented.

## Provider And Version Constraints

- Keep reusable module provider constraints minimal, tested, and explained when non-obvious. Patch-level Terraform floors or exact-looking provider floors should name the required feature or provider bug fix, or align to the repo baseline.
- Avoid reusable-module upper bounds such as `< 2.0.0` unless there is a known incompatibility or central repo policy. Root modules and callers are usually the better place for broad provider-major rollout control.
- Child modules should declare provider source and version requirements for providers they use, but only declare `configuration_aliases` for aliases the implementation actually references.

## Comments And Workarounds

- For platform IaC comments that document bugs, timing-sensitive dependencies, provider quirks, or other unusual behavior, use a concise tracked format: `// jnyjc2 YYYY-MM-DD: practical reason.` Avoid long explanatory paragraphs unless the surrounding code genuinely needs that context.
