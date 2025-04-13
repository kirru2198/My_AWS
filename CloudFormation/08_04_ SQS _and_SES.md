# Problem Statement

You work for XYZ Corporation. Your team is asked to deploy a similar architecture multiple times for testing, development, and production purposes. Implement CloudFormation for the tasks assigned to you below.

## Tasks To Be Performed:

1. **Create a FIFO SQS Queue and Test by Sending Messages:**
   - Create a FIFO (First-In-First-Out) SQS queue using AWS CloudFormation.
   - Test the functionality of the queue by sending messages to it.

2. **Register Your Mail in SES and Send a Test Email:**
   - Register your email address in Amazon Simple Email Service (SES).
   - Send a test email to yourself to verify that SES is properly configured.
--- 
To solve the problem of creating a FIFO SQS Queue and testing it, as well as registering an email in SES and sending a test email, we need to perform the following steps in AWS CloudFormation and using AWS services. Here's the solution:

### Part 1: Create a FIFO SQS Queue and Test by Sending Messages

#### CloudFormation Template for FIFO SQS Queue
We'll create a CloudFormation template to create a FIFO SQS queue.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyFifoQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: MyFifoQueue.fifo
      FifoQueue: true
      ContentBasedDeduplication: true
```

Explanation:
- **QueueName**: Specifies the name of the FIFO queue. The `.fifo` suffix is mandatory for FIFO queues.
- **FifoQueue**: Set to `true` to specify that this is a FIFO queue.
- **ContentBasedDeduplication**: Set to `true` to enable deduplication based on the content of the message (AWS will automatically deduplicate messages with the same content sent within 5 minutes).

#### Testing FIFO Queue

1. **Deploy the CloudFormation stack**:
   - Use the AWS Console, AWS CLI, or AWS SDK to deploy the CloudFormation stack with the template above.
   - For CLI, you can use:
     ```bash
     aws cloudformation create-stack --stack-name MyFifoQueueStack --template-body file://fifo-queue-template.yaml
     ```

2. **Send a message to the FIFO queue**:
   After the stack is created, you can send messages to the FIFO queue for testing.

   - Get the ARN (URL) of the FIFO queue:
     ```bash
     aws sqs get-queue-url --queue-name MyFifoQueue.fifo
     ```

   - Send a message:
     ```bash
     aws sqs send-message --queue-url <your-fifo-queue-url> --message-body "This is a test message" --message-group-id "test-group"
     ```

3. **Verify the message**:
   To confirm that the message was sent successfully:
   ```bash
   aws sqs receive-message --queue-url <your-fifo-queue-url>
   ```

### Part 2: Register Your Mail in SES and Send a Test Email

#### 1. Register Your Email Address in SES
To use Amazon SES, you need to verify the email address you want to send emails from.

1. **Verify Email in SES**:
   You can verify your email address using the AWS Management Console or via CLI.

   To verify the email address using CLI:
   ```bash
   aws ses verify-email-identity --email-address "your-email@example.com"
   ```

   You will receive a verification email. Once you click on the verification link, your email address will be successfully verified.

#### 2. Send a Test Email

Once your email address is verified, you can use Amazon SES to send a test email.

1. **Send a Test Email Using CLI**:

   First, get the region you are working in. Make sure that your SES is in the appropriate region for sending emails.

   Now send an email using the following command:

   ```bash
   aws ses send-email \
     --from "your-email@example.com" \
     --destination "ToAddresses=recipient-email@example.com" \
     --message "Subject={Data=Test Email,Charset=utf-8},Body={Text={Data=This is a test email sent via Amazon SES,Charset=utf-8}}"
   ```

   - Replace `"your-email@example.com"` with the verified email address you want to send from.
   - Replace `"recipient-email@example.com"` with the recipient's email address.
   - Modify the subject and body as needed.

2. **Check the Recipient's Email**:
   After sending the test email, check the recipient's inbox for the email.

#### CloudFormation Template for SES Setup (Optional)

Note: SES is typically configured manually through the AWS Console for the initial setup (email verification), but you can automate some parts of SES setup via CloudFormation. However, email sending via SES is not directly supported in CloudFormation, and it requires manual verification.

Here’s an example of SES email identity verification using CloudFormation:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  SESVerifiedEmail:
    Type: AWS::SES::EmailIdentity
    Properties:
      EmailIdentity: "your-email@example.com"
```

However, this will only verify the email identity. Sending the email still needs to be done using the AWS CLI, SDK, or Lambda.

### Final Summary
- **SQS FIFO Queue**: We created a FIFO queue with deduplication enabled and tested it by sending a message via the AWS CLI.
- **SES Email Registration**: We registered a verified email using AWS SES and sent a test email using the AWS CLI.

This solution fulfills the requirements of deploying a FIFO SQS queue and testing it, as well as registering an email in SES and sending a test email.
