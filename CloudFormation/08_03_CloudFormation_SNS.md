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

Sure! Here's a clear **overview** of both CloudFormation tasks and a summary of the steps happening in the code:

---

## ✅ **CloudFormation Task 1 – Overview**
### 🎯 Goal:
- Create an S3 bucket named `intellipaat-<yourname>`.
- Enable versioning on that bucket.

### 🔧 Code Summary:
- **Parameter**: Takes your name as input to generate a custom bucket name.
- **Resource**: Creates an `AWS::S3::Bucket`.
- **Versioning**: Enabled via the `VersioningConfiguration` property.
- **Output**: Displays the bucket name after stack creation.

---

## ✅ **CloudFormation Task 2 – Overview**
### 🎯 Goal:
- Extend the template from Task 1 to include **SNS notifications** for CloudFormation stack events.

### 🔧 Code Summary:
- **Uses previous template**: Reuses the S3 bucket creation and versioning logic.
- **Adds SNS Topic**:
  - Creates an `AWS::SNS::Topic`.
  - Subscribes an email address to it.
- **Adds SNS Topic Policy**: Grants permissions for publishing.
- **Notification Setup**: You receive an email to confirm the subscription.
  
> ⚠️ *Note*: This template sets up SNS and subscription but does **not** automatically notify for **every step of the stack creation** (you'd need CloudWatch Events for that). Want to include that next?

---

Let me know if you'd like me to generate the CloudWatch Event Rule to complete **real-time notifications for every stack step**.

---
Great question! Let's go over **how you can check and verify** that everything is working correctly — for **both tasks**.

---

## ✅ **Checking CloudFormation Task 1** (S3 bucket creation with versioning):

### ✔️ Option 1: Via AWS Console

1. **Go to S3 Console**:  
   [https://s3.console.aws.amazon.com/s3](https://s3.console.aws.amazon.com/s3)
   
2. **Search for your bucket**:  
   Look for a bucket named `Intellipaat-<yourname>`. For example: `Intellipaat-JohnDoe`

3. **Click on the bucket**

4. Go to the **"Properties"** tab

5. Scroll down to **“Bucket Versioning”**  
   Make sure it says: ✅ **Enabled**

---

### ✔️ Option 2: Use AWS CLI (optional)

```bash
aws s3api get-bucket-versioning --bucket Intellipaat-<yourname>
```

You should get something like:

```json
{
  "Status": "Enabled"
}
```

---

## ✅ **Checking CloudFormation Task 2** (SNS notifications):

### ✔️ Step 1: Confirm you got the **subscription email**
- You should’ve received a **“Confirm subscription”** email from AWS SNS
- If not, go to the **SNS Console** > **Topics** > Select your topic > check "Subscriptions"

---

### ✔️ Step 2: Check SNS Topic and Subscription

1. Go to the **SNS Console**:  
   [https://console.aws.amazon.com/sns](https://console.aws.amazon.com/sns)

2. Click on **Topics**, find:  
   `Intellipaat-StackNotification-<yourname>`

3. Click on the topic and go to the **“Subscriptions”** tab

4. Make sure your email is listed and its status is: ✅ **Confirmed**

---

### ✔️ Step 3: Trigger a Stack Update to Test Notifications

To see if notifications work:
1. Go back to **CloudFormation**
2. Select your stack
3. Click **Update**
4. Choose **Replace current template**, upload the same file again (even unchanged)
5. Click through and finish update

☑️ If everything works, you should receive an **email from AWS SNS** about the stack update event.

---

Ah yes — that “No updates are to be performed” message means that CloudFormation looked at your stack and saw that the template hasn't changed, so it didn’t do anything.

But no worries — there are **a few easy ways to force a change** so that the stack updates and triggers the SNS notification.

---

## ✅ Option 1: Make a Small Change to the Template

Edit your YAML template with a **tiny, harmless change** — like adding a tag to the bucket.

### 🔧 Example: Add a Tag to the S3 Bucket

Modify your `MyS3Bucket` resource like this:

```yaml
MyS3Bucket:
  Type: AWS::S3::Bucket
  Properties:
    BucketName: !Sub Intellipaat-${YourName}
    VersioningConfiguration:
      Status: Enabled
    Tags:
      - Key: Updated
        Value: True
```

This change won’t break anything — but it **will force a stack update**.

---

## ✅ Option 2: Add a Dummy Resource

If you don’t want to touch your S3 config, just add a fake/dummy resource like a **CloudFormation WaitConditionHandle**.

```yaml
DummyHandle:
  Type: AWS::CloudFormation::WaitConditionHandle
```

This resource won’t actually do anything, but it will force a change.

---

## ✅ Deploying the Update

Once you’ve made your small change:

1. Go to **CloudFormation > Select your stack**
2. Click **Update**
3. Choose **“Replace current template”** and upload the new version
4. Click through and **Create/update**

---

## ✅ Then Check for Notifications

Once the update starts, you should:

- Receive **one or more emails** from SNS during the stack update process
- See progress in **CloudFormation Events tab**

---

Want me to send you a modified YAML template with this small change added?
