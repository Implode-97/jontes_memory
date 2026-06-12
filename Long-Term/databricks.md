# Databricks Platform Memory

## Team Identities

- The platform is moving from one broad group per team toward account-level team identities with nested role groups.
- Intended role names include `_owner`, `privileged_contributor`, `contributor`, `new_contributor`, `reader`, and `consumer`, so teams can decide who belongs in each role.
- IaC should handle groups that may already exist from SCIM or IdP sync and may create empty account-level groups when they are not synced yet.
- Workspace assignment is a separate feature boundary. Do not collapse role groups into one team group or make identity ownership imply workspace membership.
- Prefer underscore `snake_case` for Databricks team keys, role group names, YAML registry fields, and generated IaC-facing identifiers unless a provider or existing resource contract requires another format.
- For account group membership modules, preserve the explicit lookup-only boundary: resolve existing target groups, users, nested groups, and service principals; create typed `databricks_group_member` resources; and do not create identities, entitlements, Unity Catalog permissions, or polymorphic membership abstractions in that helper.
- Account group naming validation belongs closest to the wrappers that derive group display names. Leaf group modules can enforce non-null, no-whitespace, and length hygiene, but dense regexes for team roles, workspace entitlement profiles, or legacy groups should stay aligned with current generators and tests.
- Team identity Terragrunt guards should preserve fail-closed lifecycle protection, final-row deletion protection, prior-state lookup through `terraform output -json`, and all-violations reporting. Cleanup pressure should stay narrow: duplicated input-shape validation, dense jq state matrices, unsupported comment stripping for generated JSON, hidden missing-output bootstrap semantics, and wrappers that use `exec` after installing an `EXIT` cleanup trap.
- For least-privilege Databricks group workflows, prefer IdP-managed groups or workspace-admin UI/direct REST over account-admin Terraform when possible. As of June 2026, workspace admins and group managers can use workspace-domain account SCIM proxy paths such as `/api/2.0/account/scim/v2/`, while the Terraform provider's account group resources and `databricks account groups-v2` CLI commands target account-host SCIM APIs and should still be treated as account-admin automation unless proven otherwise in the target account and provider/CLI version.

## Sandbox Catalogs

- Each team normally gets one sandbox catalog in the dev workspace. Additional per-region sandboxes are allowed only when transfer costs or regional GPU constraints justify them.
- Sandbox catalogs are built from S3 root storage, storage credential, external location, and catalog root configuration.
- Databricks external-location validation can race AWS IAM propagation if it checks an S3 role within milliseconds of policy or trust creation. For sandbox catalog modules and wrappers, use an intentional readiness delay or retry when CI shows correct credential and policy shape but Databricks reports missing S3 read access immediately after IAM creation.
- Fresh workspace/metastore Terragrunt E2E can still hit Unity Catalog readiness lag after Terraform-visible `databricks_metastore_assignment` completion. Treat large pre-sandbox sleeps as diagnostic evidence; prefer bounded polling of account metastore assignment, workspace current-metastore visibility, and storage credential or external-location validation for the exact target URL once the IAM role exists.
- For Terraform-managed sandbox catalogs, set the catalog owner to the platform or regional metastore owner group and grant the team owner/admin principal `ALL_PRIVILEGES`. `ALL_PRIVILEGES` does not include `MANAGE`, so the team can broadly operate in the catalog without receiving grant-management, ownership-transfer, or delete authority that can break Terraform cleanup.
- Ordinary principals need catalog ownership, or both `MANAGE` and `USE CATALOG` on the catalog, to delete it. Metastore-level `CREATE CATALOG`, account admin, workspace admin, and catalog `ALL_PRIVILEGES` are not equivalent catalog-delete authority unless the principal is also a metastore admin or has catalog-scoped authority.
- Do not half-enable Databricks file events in Terraform modules. If file events are supported, implement `enable_file_events`, the required `file_event_queue` configuration, matching S3 notification and SNS/SQS IAM permissions, tests, and cost/governance notes together; otherwise leave file events out or disabled rather than setting only the boolean or only broad `csms-*` permissions.
- They are dev-workspace collaboration areas, not productionized data products. Do not grant other teams access by default.
- The owning team role should create and manage schemas and tables, while individual object owners can decide how broadly to share child objects within the team.
- Keep region-specific sandbox declarations in that region's YAML and avoid cross-region entries unless the use case explicitly requires them.

## Unity Catalog Workspace Bindings

- `workspace_ids` owns the caller-facing resource access contract. A non-empty set means selected-workspace mode for exactly those IDs, while an empty set is valid all-workspaces/open mode and should set the securable isolation mode open instead of only omitting binding resources.
- Workspace-level providers automatically bind the creating workspace for isolated securables. Lower modules should own the Databricks-specific subtraction/anchor logic, using `provider_workspace_id` or `workspace_binding_anchor_id` only to model that implicit binding and validate selected-workspace mode; wrappers and Terragrunt callers should still pass the unmutated intended `workspace_ids` set.
- Leave Databricks' automatic provider/anchor workspace binding unmanaged. Manage explicit `databricks_workspace_binding` resources only for remaining selected workspaces, make grants use the same anchor, and force replacement when the anchor changes so stale automatic bindings do not survive.
- Separate explicit binding resources are a useful lifecycle edge for non-anchor workspaces: Terraform creates the securable first and bindings after it, then destroys explicit bindings before deleting the securable. Do not explicitly manage the automatic provider/anchor binding, because removing that access before deleting the securable can break teardown.
- Put the selected/open workspace contract and provider-workspace invariant near `workspace_ids` and `provider_workspace_id` variable descriptions. Otherwise the provider workspace ID looks arbitrary, especially when `workspace_ids = []`.
- For AWS storage credentials, document the external-ID bootstrap accurately: create a placeholder trust, create the Databricks storage credential, read the returned external ID, then finalize IAM trust and self-assumption policy. Wrapper modules may use `skip_validation = true` for credential creation while later external-location validation remains meaningful.
- For catalog, external-location, and storage-credential module outputs, preserve narrow bootstrap and wiring facts such as `name`, `external_id`, explicit `workspace_bindings`, validation flags, and lifecycle/destroy markers. Avoid duplicate broad provider-shaped objects unless a named wrapper or Terragrunt consumer needs them.

## Catalog Renames

- Stable Terragrunt registry keys, such as UUIDs separate from catalog names, can preserve Terraform resource addresses across Databricks catalog renames, but they do not by themselves solve provider state. As of June 2026, Databricks exposes catalog rename support in the platform/API SDKs, while the Terraform provider still treats `databricks_catalog.id` as name-based and has known rename failure risk.
- For catalog rename support, recommend explicit migrations: rename through API/UI when required, then remove/import Terraform state for the catalog and any dependent name-keyed resources. Avoid hiding rename behavior in a generic module until the provider has tested native rename support.

## Workspace Assignments And Entitlements

- Databricks workspace assignment wrappers should keep account and workspace provider responsibilities explicit. The default/account provider can own account groups and `databricks_mws_permission_assignment`; a `databricks.workspace` alias is appropriate only for workspace group lookup and entitlement resources.
- As of the June 2026 review notes, Databricks system-group entitlements are moving toward locked behavior: `users` entitlement hardening through Terraform is a legacy or migration concern, not a durable workflow to build new public contracts around. Before changing this area, re-check current Databricks docs.
- Preserve `workspace_team_assignments` as the stable downstream contract for workspace-scoped consumers such as cluster or compute policy modules. Treat `workspace_users_baseline`, entitlement diagnostics, provider-owned assignment IDs, and membership-resource IDs as diagnostic or test-shaped unless real consumers need them.
- For team role assignment defaults, the useful semantic mapping is `consumer -> workspace_consume`, `reader -> sql_access`, and `contributor -> workspace_access`; avoid duplicating dev/prd mock fixtures unless the environment changes that contract.
- Workspace assignment Terragrunt guards should preserve the account/workspace provider split, workspace dependency, team-identity ordering, generated lifecycle output, and row-removal protection. The registry invariant is that active placement rows (`enabled` or `destroy_pending`) reference enabled team rows, while destroyed placements are ignored.
- Keep workspace-assignment generated input validation self-contained at every hook that consumes it. Either run the required-input guard before validate-time registry checks or make the registry guard validate the `teams` and `workspace_teams` array shapes itself; do not let missing or malformed `workspace_team_lifecycle_states.value` look like a proven first apply.

## Data Products

- Productionized Databricks data products should be managed by a dedicated service principal tied to the product's GitLab repository, preferably through OIDC or federation instead of long-lived raw tokens.
- DDS Simulation ingestion Databricks token-expiry support is tracked by Jira `CAV-134024` under Firecrackers Support PI2616. `firecrackers-sp` was inferred as the deployment service principal; a Databricks token was created and uploaded to the GitLab CI/CD variables, and the story was resolved as Done while Nishant's retry or merge-path confirmation was still pending. Treat the support action as complete unless Nishant reports the rerun still fails.
- Team groups should not directly control productionized products. Development happens in the team's sandbox; staging or UAT validates changes before production; jobs normally run in the prod workspace for prod targets.
- Catalog naming examples use `dp_<product>_<target>`, such as `dp_car_images_prd` or `dp_car_images_uat`, while leaving room for SIT/UAT variants.
- Model repo-bound service principals, federation policy, catalog ownership, and target environments explicitly instead of letting team group permissions blur the lifecycle.

## Cluster Policies

- Cluster policy defaults should be reusable configurations, not one shared policy granted to everyone.
- Generate or bind policies per team, role, service principal, or product principal so mandatory tags remain accurate.
- Mandatory tags should include owning team, billing cost center, principal name, principal type, data product, and deployment target.
- Product teams may add finer-grained tags, but they must not overwrite platform cost tracking. Keep optional free-text job tags out of platform-managed core unless the platform can validate them safely.
- Default policy profiles should be platform-owned, with additional larger policies declared separately and intentionally bound.
- For compute policy modules, keep deterministic policy rendering separate from output shaping: module-owned JSON rules, hidden `custom_tags.*` injection, and separate `CAN_USE` permissions are valuable; broad `policy_assignments` or rendered-policy outputs need observed consumers before becoming public API.
- In the current team identity hierarchy, `owner` is nested into `privileged_contributor`, which is nested into `contributor`, then lower roles. For team sandbox compute policies, granting `CAN_USE` to `grp_team_<team_key>_contributor` covers contributor, privileged contributor, and owner members; duplicate grants to higher nested role groups are usually noise.
- Split automatic low-cost default compute policies from higher-cost, ML-oriented, or otherwise special policies. Defaults should arrive automatically when a team is assigned to a workspace; explicit policies should be additive, approved separately, and modeled with distinct profile or policy identities rather than as overrides of the default path.

## External Storage

- The platform goal is to remain the single governance surface for data.
- For enterprise MLOps and autonomous-driving training, treat Unity Catalog as the source of truth for authorization, discovery, lineage, and model/data governance, not as a literal ban on all physical S3 access. Direct access to UC managed storage remains a hard no, but the platform should offer governed paved roads such as UC tables and volumes for discovery/manifests, credential vending or Open APIs where supported, regional AWS training stacks such as SageMaker, HyperPod, or EKS with tightly scoped short-lived or brokered S3 access, and write-back of lineage, artifacts, and models into UC/MLflow.
- Challenge blanket S3 bans that lack usable MLOps paths. Prefer productized exceptions with audit, TTL, owner, tags, and registry-driven IAM, S3, or Lake Formation controls over ad hoc roles or direct bucket grants.
- For team-owned external S3 storage, prefer read-oriented ingestion into managed catalogs rather than letting external storage become an uncontrolled write surface.
- External locations and volumes should respect ownership, workspace binding, and read-only constraints where the feature is explicitly for reference or ingestion.
- Writes for governed platform data should land in platform-managed catalogs and storage unless the user explicitly changes the governance model.

## AWS Networking For Databricks

- For Databricks AWS customer-managed VPC security groups, verify current Databricks docs before freezing ports. The June 2026 review notes treated self TCP/UDP, outbound 443, 3306, 8443, 8444, 8445, reserved 8446-8451, and conditional Lakebase 5432 as the relevant contract.
- Security group modules should keep separate rule resources for state clarity, use stable caller-chosen keys for custom egress rules, validate IPv4 CIDRs when using `cidr_ipv4`, and avoid broad future-port toggles unless the platform intentionally supports those ports now.

## Apps And User Pass-Through

- In Databricks Apps, default SDK clients such as `WorkspaceClient()` use the app service principal from injected environment variables. User pass-through requires explicitly using the forwarded `x-forwarded-access-token`.
- When debugging app data-access failures, trace every SQL Statements call, Files API call, Unity Catalog table read, volume-file read, and helper fallback to its exact credential source. Mixed SDK default auth, direct REST calls with user tokens, and Streamlit caches can make path discovery and file reads run under different principals.
- When user authorization is intended, prefer fail-fast diagnostics over silent service-principal fallback. Check declared user scopes, app resource grants, and any cached per-user tokens before treating a UC or volume read failure as a table, path, or Spark issue.
- For the Databricks App volume image-loading user-token auth support trace, use Jira `CAV-133791`; it is under Firecrackers Support PI2616 and records the explicit forwarded-token `WorkspaceClient(..., auth_type="pat")` fix.

## Platform Bootstrap And Admin Groups

- Account-level Databricks administration and Unity Catalog object privileges are distinct. A deployer service principal may be able to create account-level resources without automatically owning or being able to create objects inside the metastore after IaC assigns metastore ownership elsewhere.
- For metastore bootstrap, add the current IaC deployer service principal to the existing metastore admin group instead of making the deployer the metastore owner.
- For account-scoped wrappers, pass an explicit `deployer_service_principal_application_id` from account or environment config and resolve it through account-compatible Databricks service-principal data sources. Do not rely on `databricks_current_user` with an account-scoped provider because it requires workspace context and fails without a `workspace_id`.
- Source the deployer application/client ID from the CI identity, normally `DATABRICKS_CLIENT_ID`, rather than a display-name lookup or one generic global service-principal ID. If multi-account deployment becomes real, use per-account CI environments or account-scoped live config.
- The workspace wrapper should create a platform-prefixed workspace admin group such as `platform-<workspace_name>-workspace-admins`, assign it `ADMIN` on the workspace, and include the current deployment service principal. Keep `admin` for permission-bearing platform groups; do not overload team role names such as `owner`.
- For workspace bootstrap wrappers, explicit `depends_on` is justified when it protects hidden Databricks readiness, such as permission assignment only becoming available after metastore assignment enables identity federation. Name the exact hidden readiness in comments, and replace broad module-level dependencies only when child modules expose precise readiness outputs.

## Workspace Permission Ordering

- New Databricks workspaces can reject `databricks_mws_permission_assignment` with `Permission assignment APIs are not available for this workspace` when permission assignment races ahead of metastore assignment and identity federation.
- Workspace admin group or principal assignment should depend on the full workspace module or metastore-assignment completion, not only on `module.workspace.workspace_id`, because the workspace ID can be known before the child module has finished enabling identity federation.
- If the same error, or downstream workspace-level UC API failures, persist after explicit dependency ordering, investigate Databricks eventual consistency and poll readiness before broadening the module contract.

## Real-Provider Tests

- Real-provider Databricks tests should use pinned IDs for actual shared test resources, not all-zero or made-up UUID placeholders.
- In the shared Databricks account-level module test environment for `data-platform-terraform-modules`, the deployer service-principal application/client ID is `f425e414-e206-48d5-80ab-6d537930d0d7`.
- For the sandbox catalog wrapper real apply test in `data-platform-terraform-modules`, use the shared dev workspace as a test fixture: host `https://dbc-b72ad987-f679.cloud.databricks.com`, workspace ID `4022911453310458`, Databricks account ID `47dc11d5-0e29-4a29-b9b0-9d3d3961e518`, region `eu-west-1`, and metastore ID `3f86dad4-0a1b-4efb-9bcd-e32a1a541300`. Prefer these as test defaults over required CI `TF_VAR_databricks_workspace_host` or `TF_VAR_databricks_workspace_id` wiring for that real test.

## Spark Performance

- For I/O-bound Spark workloads reading Unity Catalog Volumes, start with config-level and file-layout levers when practical: file scan partition sizing, file open cost, input partition count, parallel file discovery, shuffle partitioning only when relevant, and current Databricks runtime defaults.
- Distinguish Spark file-source reads from custom code opening `/Volumes/...` paths inside tasks. Spark file-source configs only help the Spark read path; Delta/table layout conversion or per-task threaded I/O may be the realistic fix for custom file reads.
- `spark.task.cpus` cannot be fractional, and Spark cannot oversubscribe task slots per CPU through that setting.

## Serverless GPU / AI Runtime

- Databricks Serverless GPU is exposed as AI Runtime on Databricks Serverless, not as classic GPU clusters, Databricks Runtime ML, cluster policies, init scripts, or Docker images.
- Platform users choose Serverless GPU / AI Runtime from notebooks or jobs, then select an accelerator such as A10 or H100 and a managed serverless environment version.
- Current v5 environment notes include Ubuntu 24.04.2, Python 3.12.3, Databricks Connect 18.0.0, and CUDA Toolkit 12.9. The v5 Standard/base GPU environment is intentionally minimal and does not include PyTorch; use AI v5 or explicit `%pip` / environment dependencies for ML libraries.
- Treat serverless GPU as a governed, preview, region-limited capability controlled through previews, usage policies, tags, environment dependency standards, and documented notebook/job templates.
