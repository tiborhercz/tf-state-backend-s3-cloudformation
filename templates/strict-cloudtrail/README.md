# CloudFormation: S3 state backend for Terraform

This CloudFormation template focuses on security. The features of this CloudFormation template are:

- CloudTrail logging
- Encrypted bucket with KMS (enforced)
- Encrypted DynamoDB table
- Terraform admin role with access to the Terraform state file
- Terraform deployment role with access to the Terrafrom state file
- All public access is blocked
- S3 bucket versioning

# Parameters explained
Below you will find the parameters explained

## EnforceMfaEnabled
MFA enforcement can be toggled.
Why to disable MFA enforcement
1. When using the Terraform state backend with a CICD pipeline it can be difficult to have the CICD pipeline authenticate with MFA.
2. when using assumed roles through SSO the `aws:MultiFactorAuthAge` and `aws:MultiFactorAuthAge` are not present in the request. Therefore, you should disable the MFA enforcement.

## AdminRoleId parameter
The parameter `AdminRoleId` needs to be a role ID. 
Which you can retrieve by executing the following command: `aws iam get-role --role-name ROLE_NAME_HERE`. 
The role ID looks like: `AROAEXAMPLEID`.

# Deploy instructions

Deploy state bucket and DynamoDB table
```
aws cloudformation create-stack --region <REGION-B> --stack-name terraform-state-backend --template-body file://state-backend-s3.yml
```