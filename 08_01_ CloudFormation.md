# CloudFormation Problem Statement

## Background
You work for XYZ Corporation. Your team is tasked with deploying similar architecture multiple times for testing, development, and production purposes. Implement CloudFormation for the tasks assigned to you below.

## Tasks to be Performed

1. **Create a template that can create an S3 bucket named:**
   - `Intellipaat-<yourname>`

2. **Enable versioning for the S3 bucket created.**
---
To create an AWS CloudFormation template that deploys an S3 bucket named `Intellipaat-<yourname>` and enables versioning, you can follow the YAML format for CloudFormation. Below is the CloudFormation template for the required task:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: CloudFormation template to create an S3 bucket with versioning enabled

Resources:
  S3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'Intellipaat-${AWS::AccountId}'
      VersioningConfiguration:
        Status: Enabled

Outputs:
  S3BucketName:
    Description: Name of the S3 bucket
    Value: !Ref S3Bucket
```

### Explanation:
- **AWSTemplateFormatVersion**: Specifies the version of the CloudFormation template format. `'2010-09-09'` is the current version.
- **Description**: A brief description of what this template does.
- **Resources**: This section defines the resources to be created.
  - **S3Bucket**: The type of resource is an S3 bucket (`AWS::S3::Bucket`).
  - **BucketName**: Uses the `!Sub` function to dynamically name the bucket. The bucket name is prefixed with `Intellipaat-` and then the `AWS::AccountId` intrinsic function ensures that the bucket is unique to your AWS account.
  - **VersioningConfiguration**: The versioning for the bucket is enabled with the `Status: Enabled` property.
- **Outputs**: This section outputs the name of the created S3 bucket after stack creation.

### Important:
- Replace `<yourname>` with your actual name in the description if needed or leave it as a placeholder if you want it to automatically use the account ID for uniqueness.
  
### Deploying the Template:
1. Save the above code to a `.yaml` file (e.g., `s3_bucket_with_versioning.yaml`).
2. Use AWS Console, AWS CLI, or AWS SDK to deploy the CloudFormation stack.
   - For AWS CLI:
     ```bash
     aws cloudformation create-stack --stack-name MyS3Stack --template-body file://s3_bucket_with_versioning.yaml
     ```

This CloudFormation template will create the S3 bucket with versioning enabled for your testing, development, or production environment.
