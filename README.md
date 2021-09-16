# CloudFormation: S3 state backend for Terraform

---

CloudFormation template to provision a S3 bucket to store the `terraform.tfstate` file
and a DynamoDB table to lock the state file to prevent concurrent modifications and state corruption.

### Template features:

- S3 server-side encryption at rest
- S3 bucket versioning
- DynamoDB service-side encryption
- Replication bucket in other region
- All public access is blocked
- No cross-account support

### Bucket policies

- Deny unencrypted object upload
- Deny object deletion
- Deny not using TLS

## Deploy the CloudFormation Templates

---

AWS CloudFormation templates cannot (without the use of StackSets) be deployed across multiple regions in one template
that is why you will find two templates in the repo.

The main (source) bucket depends on the replication (destination) bucket, and therefore you have to deploy the
replication bucket first.

**Deploy order:**

1. `state-backend-s3-replication.yml`
2. `state-backend-s3.yml`

Provision both templates in different regions within the same account

## Example Terraform configuration

---

```terraform
terraform {
  backend "s3" {
    bucket         = "BucketName"
    dynamodb_table = "DynamoDbTableName"
    key            = "path/to/my/key"
    region         = "eu-west-1"
    encrypt        = true
  }
}
```

