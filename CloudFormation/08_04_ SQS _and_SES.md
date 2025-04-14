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

## Step 2: Test by Sending Messages
1. After the queue is created, click its name to open the queue details.
2. Under the "Send and receive messages" tab:
- Enter a Message body (e.g., "Hello from FIFO queue!")
- Enter a Message group ID (required for FIFO queues, e.g., "testGroup1").
- Click Send message.

3. To verify:
- Scroll down and click Poll for messages to receive messages.
- You should see the message you just sent.
✅ FIFO queue creation and testing complete.

---

## ✅ **Task 2: Register Your Mail in SES and Send a Test Email**

### Step 1: Verify Email Address in SES
1. Go to **Amazon SES** in the AWS Console.
2. In the left menu, click **"Verified identities"**.
3. Click **"Create identity"**.
4. Choose **"Email address"**.
5. Enter your email (e.g., `youremail@example.com`) and click **"Create identity"**.
6. You'll receive a **verification email** at the address.
7. Open the email and **click the verification link**.

   ✅ Once verified, SES will show your email as “Verified.”

### Step 2: Send a Test Email
1. Go to **SES → Email test** (under "Email sending" or use the verified identity page).
2. Choose:
   - **From**: Your verified email
   - **To**: Same or another verified email (if still in sandbox)
   - Enter subject and body.
3. Click **Send test email**.

📌 *Note*: SES starts in **sandbox mode**, meaning you can only send to **verified addresses**. To send to unverified addresses, you’ll need to request **production access**.

---

## Let Me Know
Would you also like the **CloudFormation YAML template** for the FIFO queue, even though you used the console? It’s useful for reusability across environments like dev, test, and prod.
