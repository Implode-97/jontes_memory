# AWS Project Memory

## Account And Profile Safety

- For Data Platform and dmp-dev AWS diagnostics, prefer a dev-scoped AWS profile and verify caller identity before running AWS CLI checks.
- The key safety boundary is account scope: read-only use of local profiles such as `catalog-deploy` or `scania-bedrock` can be acceptable when `aws sts get-caller-identity` shows dmp-dev account `498873881431`, even if the role is Administrator inside that account.
- Do not silently fall back to an admin identity that can operate across all AWS accounts. State the selected profile, account, and role in working notes, and keep investigations read-only unless the user explicitly approves changes or escalation.
