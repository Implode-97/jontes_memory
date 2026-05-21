# Terraform Module Preferences

## Provider Facts And Inputs

- Avoid passing account, region, Databricks account, and similar provider-owned facts as ordinary inputs when they can be reliably derived from configured providers, required IDs, or data sources.
- Prefer `aws_caller_identity`, `aws_region`, Databricks current-config style data sources, and metastore lookups when those values are part of provider context.
- Hard-code the normal commercial AWS partition as `aws`; do not add `aws_partition` inputs or `data.aws_partition` just to support China or GovCloud unless the user explicitly changes that enterprise assumption.
- For Databricks wrappers, the current IaC deployer service principal is provider-owned context. Derive it with provider data sources such as `databricks_current_user` plus service-principal lookup instead of adding live or Terragrunt inputs for the deployer identity.

## Module Boundaries

- Keep lower-level Terraform modules such as secure S3 bucket, IAM role, and security group modules generic enough for non-Databricks use.
- Put Databricks-specific policy documents, descriptions, pass-role defaults, workspace wiring, and root-storage semantics in Databricks wrapper modules or Terragrunt composition.
- If a reusable AWS module contains hard-coded Databricks language, move that specificity into the wrapper unless the module contract is truly Databricks-specific.
- Keep module contracts narrow until real requirements appear. Do not add fake flexibility for arbitrary principals, regions, accounts, or ownership variants before there is a concrete use case.

## Testing Confidence

- Prefer real provider plan or apply tests where they give meaningful confidence and are practical to run.
- For ordinary resource modules, real apply tests are preferred when credentials and environment allow it. For expensive or slow resources such as full Databricks workspaces or broad wrappers, use real provider plan tests and mock-provider apply tests.
- Do not rely on `prevent_destroy` as a cleanup safety net in tests; if a resource is removed from code, Terraform no longer enforces that lifecycle rule.
- Focus Terraform tests on contract, naming, policy shape, invalid inputs, and lifecycle-sensitive behavior. State clearly when validation is limited to plan or mocks.
