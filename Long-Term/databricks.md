# Databricks Platform Memory

## Team Identities

- The platform is moving from one broad group per team toward account-level team identities with nested role groups.
- Intended role names include `_owner`, `privileged_contributor`, `contributor`, `new_contributor`, `reader`, and `consumer`, so teams can decide who belongs in each role.
- IaC should handle groups that may already exist from SCIM or IdP sync and may create empty account-level groups when they are not synced yet.
- Workspace assignment is a separate feature boundary. Do not collapse role groups into one team group or make identity ownership imply workspace membership.
- Prefer underscore `snake_case` for Databricks team keys, role group names, YAML registry fields, and generated IaC-facing identifiers unless a provider or existing resource contract requires another format.

## Sandbox Catalogs

- Each team normally gets one sandbox catalog in the dev workspace. Additional per-region sandboxes are allowed only when transfer costs or regional GPU constraints justify them.
- Sandbox catalogs are built from S3 root storage, storage credential, external location, and catalog root configuration.
- They are dev-workspace collaboration areas, not productionized data products. Do not grant other teams access by default.
- The owning team role should create and manage schemas and tables, while individual object owners can decide how broadly to share child objects within the team.
- Keep region-specific sandbox declarations in that region's YAML and avoid cross-region entries unless the use case explicitly requires them.

## Data Products

- Productionized Databricks data products should be managed by a dedicated service principal tied to the product's GitLab repository, preferably through OIDC or federation instead of long-lived raw tokens.
- Team groups should not directly control productionized products. Development happens in the team's sandbox; staging or UAT validates changes before production; jobs normally run in the prod workspace for prod targets.
- Catalog naming examples use `dp_<product>_<target>`, such as `dp_car_images_prd` or `dp_car_images_uat`, while leaving room for SIT/UAT variants.
- Model repo-bound service principals, federation policy, catalog ownership, and target environments explicitly instead of letting team group permissions blur the lifecycle.

## Cluster Policies

- Cluster policy defaults should be reusable configurations, not one shared policy granted to everyone.
- Generate or bind policies per team, role, service principal, or product principal so mandatory tags remain accurate.
- Mandatory tags should include owning team, billing cost center, principal name, principal type, data product, and deployment target.
- Product teams may add finer-grained tags, but they must not overwrite platform cost tracking. Keep optional free-text job tags out of platform-managed core unless the platform can validate them safely.
- Default policy profiles should be platform-owned, with additional larger policies declared separately and intentionally bound.

## External Storage

- The platform goal is to remain the single governance surface for data.
- For team-owned external S3 storage, prefer read-oriented ingestion into managed catalogs rather than letting external storage become an uncontrolled write surface.
- External locations and volumes should respect ownership, workspace binding, and read-only constraints where the feature is explicitly for reference or ingestion.
- Writes for governed platform data should land in platform-managed catalogs and storage unless the user explicitly changes the governance model.

## Platform Bootstrap And Admin Groups

- Account-level Databricks administration and Unity Catalog object privileges are distinct. A deployer service principal may be able to create account-level resources without automatically owning or being able to create objects inside the metastore after IaC assigns metastore ownership elsewhere.
- For metastore bootstrap, add the current IaC deployer service principal to the existing metastore admin group instead of making the deployer the metastore owner.
- For account-scoped wrappers, pass an explicit `deployer_service_principal_application_id` from account or environment config and resolve it through account-compatible Databricks service-principal data sources. Do not rely on `databricks_current_user` with an account-scoped provider because it requires workspace context and fails without a `workspace_id`.
- Source the deployer application/client ID from the CI identity, normally `DATABRICKS_CLIENT_ID`, rather than a display-name lookup or one generic global service-principal ID. If multi-account deployment becomes real, use per-account CI environments or account-scoped live config.
- The workspace wrapper should create a platform-prefixed workspace admin group such as `platform-<workspace_name>-workspace-admins`, assign it `ADMIN` on the workspace, and include the current deployment service principal. Keep `admin` for permission-bearing platform groups; do not overload team role names such as `owner`.

## Workspace Permission Ordering

- New Databricks workspaces can reject `databricks_mws_permission_assignment` with `Permission assignment APIs are not available for this workspace` when permission assignment races ahead of metastore assignment and identity federation.
- Workspace admin group or principal assignment should depend on the full workspace module or metastore-assignment completion, not only on `module.workspace.workspace_id`, because the workspace ID can be known before the child module has finished enabling identity federation.
- If the same error persists after explicit dependency ordering, investigate Databricks eventual consistency before broadening the module contract.

## Real-Provider Tests

- Real-provider Databricks tests should use pinned IDs for actual shared test resources, not all-zero or made-up UUID placeholders.
- In the shared Databricks account-level module test environment for `data-platform-terraform-modules`, the deployer service-principal application/client ID is `f425e414-e206-48d5-80ab-6d537930d0d7`.

## Serverless GPU / AI Runtime

- Databricks Serverless GPU is exposed as AI Runtime on Databricks Serverless, not as classic GPU clusters, Databricks Runtime ML, cluster policies, init scripts, or Docker images.
- Platform users choose Serverless GPU / AI Runtime from notebooks or jobs, then select an accelerator such as A10 or H100 and a managed serverless environment version.
- Current v5 environment notes include Ubuntu 24.04.2, Python 3.12.3, Databricks Connect 18.0.0, and CUDA Toolkit 12.9. The v5 Standard/base GPU environment is intentionally minimal and does not include PyTorch; use AI v5 or explicit `%pip` / environment dependencies for ML libraries.
- Treat serverless GPU as a governed, preview, region-limited capability controlled through previews, usage policies, tags, environment dependency standards, and documented notebook/job templates.
