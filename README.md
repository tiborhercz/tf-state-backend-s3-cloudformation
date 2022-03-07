# CloudFormation: S3 state backend for Terraform

In this repository in the 'templates' folder you will find CloudFormation templates.

The CloudFormation templates focus on different things. I will explain the templates below:

## simple-replication template

This template focuses on durability and therefore uses a replication bucket.
The template is not strict on permissions, this means administrators and users with the right IAM policies have full access to this bucket.

## strict-cloudtrail template

This template focuses on security with strict permissions. Meaning only the deployment role and Terraform admin role can access the Terraform state file.
For access logging the bucket has CloudTrail logging enabled. 

