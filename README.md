# CloudFormation: S3 state backend for Terraform

CloudFormation template to provision a S3 bucket to store the `terraform.tfstate` file
and a DynamoDB table to lock the state file to prevent concurrent modifications and state corruption.

### Template features:

- S3 server-side encryption at rest
- S3 bucket versioning
- DynamoDB server-side encryption
- Multi region support for bucket replication
- All public access is blocked
- No cross-account support

### Bucket policies

- Deny unencrypted object upload
- Deny object deletion
- Deny not using TLS

## Deploy the CloudFormation Templates

AWS CloudFormation templates cannot (without the use of StackSets) be deployed across multiple regions in one template
that is why you will find two templates in this repository.

The main (source) bucket depends on the replication (destination) bucket, and therefore you have to deploy the
replication bucket first.

### Deploy instructions

Deploy replication bucket
```
aws cloudformation create-stack --region <REGION-A> --stack-name terraform-state-replication-bucket --template-body file://state-backend-s3-replication.yml --capabilities CAPABILITY_IAM
```

Deploy state bucket and DynamoDB table
```
aws cloudformation create-stack --region <REGION-B> --stack-name terraform-state-backend --template-body file://state-backend-s3.yml --capabilities CAPABILITY_IAM
```

## Example Terraform configuration

Get the S3 bucketname and DynamoDB table name from the CloudFormation template (state-backend-s3.yml) outputs.

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

---

###### Inspired by: [qtangs gist](https://gist.github.com/qtangs/c91b5f962147d8da87be947b83d80cee)
