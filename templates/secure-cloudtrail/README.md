## CloudFormation parameters

### EnforceMfaEnabled
MFA enforcement can be toggled.
Why to disable MFA enforcement
1. When using the Terraform state backend with a CICD pipeline it can be difficult to have the CICD pipeline authenticate with MFA.
2. when using assumed roles through SSO the `aws:MultiFactorAuthAge` and `aws:MultiFactorAuthAge` are not present in the request. Therefore, you should disable the MFA enforcement.

Deployment instructions:
1. Create a KMS key and get the key ID
2. Get the ID from the admin role required to access the bucket `aws iam get-role --role-name ROLE_NAME_HERE`. The role ID looks like: AROAEXAMPLEID.