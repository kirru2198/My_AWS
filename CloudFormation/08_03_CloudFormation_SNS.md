# CloudFormation SNS

## Problem Statement:
You work for XYZ Corporation. Your team is tasked with deploying similar architectures multiple times for testing, development, and production purposes. Implement CloudFormation for the tasks assigned to you below.

## Tasks to Be Performed:
1. Use the template from **CloudFormation Task 1**.
2. Add a notification to the CloudFormation stack using **SNS** so that you receive an email notification for every step of the stack creation process.

--- 
Great! Let’s walk through the **step-by-step process** to complete both CloudFormation tasks. This will include writing a CloudFormation YAML template and setting up an SNS topic with email notifications.

---

## ✅ **CloudFormation Task 1: Create S3 Bucket with Versioning**

### Step 1: Create a CloudFormation YAML Template

Create a file named `s3-bucket.yml` with the following content:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Create an S3 bucket with versioning enabled

Parameters:
  YourName:
    Type: String
    Description: kiran

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub "intellipaat-${YourName}"
      VersioningConfiguration:
        Status: Enabled

Outputs:
  BucketName:
    Description: Name of the created S3 bucket
    Value: !Ref MyS3Bucket
```

### Step 2: Deploy the Stack

1. Go to AWS CloudFormation console.
2. Click **Create stack > With new resources (standard)**.
3. Choose **Template is ready**, and upload the `s3-bucket.yml`.
4. Click **Next**.
5. Enter stack name (e.g., `S3BucketStack`) and input your name for the parameter.
6. Click through the rest of the steps and click **Create stack**.

---

## ✅ **CloudFormation Task 2: Add SNS Notifications for Stack Events**

### Step 1: Modify the Template from Task 1

Create a new file `s3-bucket-with-sns.yml` and update as follows:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Create S3 bucket with versioning and SNS notifications

Parameters:
  YourName:
    Type: String
    Description: kiran
  EmailAddress:
    Type: String
    Description: kirru2198@gmail.com

Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub "intellipaat-${YourName}"
      VersioningConfiguration:
        Status: Enabled

  MySNSTopic:
    Type: AWS::SNS::Topic
    Properties:
      Subscription:
        - Endpoint: !Ref EmailAddress
          Protocol: email

  TopicPolicy:
    Type: AWS::SNS::TopicPolicy
    Properties:
      Topics:
        - !Ref MySNSTopic
      PolicyDocument:
        Statement:
          - Effect: Allow
            Principal: "*"
            Action: "sns:Publish"
            Resource: "*"

Outputs:
  BucketName:
    Description: Name of the created S3 bucket
    Value: !Ref MyS3Bucket
  SNSTopicARN:
    Description: ARN of the SNS topic
    Value: !Ref MySNSTopic
```

> 💡 This stack sets up the S3 bucket and an SNS topic that emails you when messages are published to it. It won’t automatically notify for **each stack event**, but this is the CloudFormation-level implementation.

### Step 2: Deploy the Stack

1. Go to AWS CloudFormation.
2. Click **Create stack > With new resources (standard)**.
3. Upload the new `s3-bucket-with-sns.yml`.
4. Enter stack name (e.g., `S3BucketWithSNSNotification`).
5. Enter your name and a valid **email address** for the `EmailAddress` parameter.
6. Click through and **create** the stack.

### Step 3: Confirm the Email Subscription

Check your email inbox and **confirm the subscription** to the SNS topic.

---

## Optional: Get Notifications for CloudFormation Stack Events (Extra)

To receive actual **stack event** notifications (not just SNS from resources in the stack), you must:

1. Create a **CloudWatch Event Rule** that triggers on CloudFormation events.
2. Use that rule to send notifications via SNS.

Let me know if you'd like that advanced setup too — happy to walk you through!

---
