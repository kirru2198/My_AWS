# CloudFormation SNS

## Problem Statement:
You work for XYZ Corporation. Your team is tasked with deploying similar architectures multiple times for testing, development, and production purposes. Implement CloudFormation for the tasks assigned to you below.

## Tasks to Be Performed:
1. Use the template from **CloudFormation Task 1**.
2. Add a notification to the CloudFormation stack using **SNS** so that you receive an email notification for every step of the stack creation process.

---
To solve this problem, you'll need to modify a CloudFormation template to include an SNS (Simple Notification Service) topic. This will send email notifications for every step of the stack creation process.

The solution involves these steps:

1. **Create an SNS Topic**: This is where the notifications will be sent.
2. **Subscribe an Email Address to the SNS Topic**: You'll need to subscribe an email to the SNS topic so you can receive notifications.
3. **Modify CloudFormation Template to Use the SNS Topic**: Integrate this SNS topic with CloudFormation stack events so that notifications are triggered when the stack goes through different creation phases.

Below is an example CloudFormation YAML template that implements this:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'CloudFormation Stack with SNS notifications'

Resources:
  # Step 1: Create an SNS Topic for notifications
  StackCreationNotificationTopic:
    Type: 'AWS::SNS::Topic'
    Properties: 
      DisplayName: 'StackCreationNotificationTopic'
      TopicName: 'StackCreationNotifications'

  # Step 2: Subscribe an Email Address to the SNS Topic
  StackCreationEmailSubscription:
    Type: 'AWS::SNS::Subscription'
    Properties:
      Endpoint: 'youremail@example.com'  # Replace with your email address
      Protocol: 'email'
      TopicArn: !Ref StackCreationNotificationTopic

  # Step 3: Set Stack Notification Configuration for CloudFormation Events
  StackEvents:
    Type: 'AWS::CloudFormation::Stack'
    Properties: 
      StackName: 'MyStack'
      NotificationARNs: 
        - !Ref StackCreationNotificationTopic

Outputs:
  SNSNotificationTopic:
    Description: 'SNS Topic ARN for Stack Creation Notifications'
    Value: !Ref StackCreationNotificationTopic
```

### Explanation of Key Sections:

1. **SNS Topic Creation (`StackCreationNotificationTopic`)**:
   - This creates an SNS topic that will be used to send notifications.
   - The `DisplayName` is a friendly name for the topic (can be anything you choose).
   
2. **SNS Subscription (`StackCreationEmailSubscription`)**:
   - This subscribes your email (`youremail@example.com`) to the SNS topic. Replace `youremail@example.com` with your actual email address.
   - When the SNS topic is triggered, an email will be sent to this address.
   
3. **CloudFormation Notification Integration (`StackEvents`)**:
   - The `NotificationARNs` property in this section integrates the SNS topic with the CloudFormation stack.
   - Whenever there is a status change in the stack (for example, `CREATE_IN_PROGRESS`, `CREATE_FAILED`, etc.), CloudFormation will publish the events to the SNS topic.
   
4. **Outputs Section (`SNSNotificationTopic`)**:
   - This outputs the ARN of the SNS topic, which can be useful for debugging or reusing in other parts of the template.

### Steps to Deploy:

1. **Save this template** as a `.yaml` file, e.g., `cloudformation_with_sns_notifications.yaml`.
2. **Deploy the CloudFormation stack** via AWS Console, CLI, or SDKs:
   - Using AWS Console, go to **CloudFormation** > **Create Stack** > **Upload a template file**.
   - In the **Upload Template** section, choose the template file and proceed with the stack creation.

3. **Confirm the Subscription**:
   - You will receive an email asking to confirm the subscription. Make sure to confirm it so that you can receive notifications.

4. **Monitor Notifications**:
   - After the stack is created, you will receive an email notification each time the stack changes state (e.g., when it starts creating, completes, or fails).

### Conclusion:
This template creates an SNS topic, subscribes an email address to it, and integrates the SNS topic with CloudFormation to receive notifications at every step of the stack creation process. Make sure to replace the placeholder email address with your own.
