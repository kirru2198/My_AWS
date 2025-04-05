# Problem Statement:

You work for **XYZ Corporation**. Your corporation wants to launch a new web-based application and they do not want their servers to be running all the time. It should also be managed by AWS. Implement suitable solutions.

## Tasks To Be Performed:

1. **Create a sample Python Lambda function**.
2. **Set the Lambda trigger as SQS** and send a message to test invocations.

---
# Solution: AWS Lambda with SQS Trigger

The solution involves using **AWS Lambda** to run code without managing servers. AWS Lambda will be triggered by messages sent to an **SQS (Simple Queue Service)** queue. Here’s how you can implement it:

## 1. **Create a Sample Python Lambda Function**

### Steps to Create a Python Lambda Function:
1. **Sign in to the AWS Management Console** and navigate to the **Lambda** service.
2. **Create a New Lambda Function**:
   - Click on **Create function**.
   - Select **Author from scratch**.
   - Provide a **Function name** (e.g., `SampleLambdaFunction`).
   - Choose **Python 3.x** as the runtime.
   - Select **Create a new role with basic Lambda permissions** to allow the Lambda function to write logs to CloudWatch.
   
3. **Write the Python Code**:
   - In the inline editor, write the Python code for your Lambda function. Here’s an example of a simple Python Lambda function:

   ```python
   import json

   def lambda_handler(event, context):
       # Print the event data received from SQS
       print("Received event: ", json.dumps(event, indent=2))

       # Process the SQS message
       for record in event['Records']:
           # Extract the message body
           message_body = record['body']
           print(f"Processing message: {message_body}")
           # Add your business logic here

       return {
           'statusCode': 200,
           'body': json.dumps('Message processed successfully')
       }
   ```

   - This function prints the event data from the SQS message and processes the message body.
   
4. **Save the Function**:
   - Click on **Deploy** to save the Lambda function.

---

## 2. **Set the Lambda Trigger as SQS**

### Steps to Set up SQS Trigger for Lambda:
1. **Create an SQS Queue**:
   - Navigate to **SQS** in the AWS Management Console.
   - Click on **Create Queue** and choose **Standard Queue**.
   - Provide a **Queue name** (e.g., `MyQueue`).
   - Leave other settings as default and click on **Create Queue**.

2. **Configure Lambda to Be Triggered by SQS**:
   - Go back to the **Lambda console** and open the function you just created.
   - Under the **Designer** section, click on **Add trigger**.
   - Choose **SQS** as the trigger.
   - Select the **SQS queue** you just created (e.g., `MyQueue`).
   - Check the box **Enable trigger** and click **Add**.
   
3. **Give Lambda Permission to Access SQS**:
   - AWS automatically adds permissions to allow Lambda to read from the SQS queue. Ensure that the Lambda role includes the necessary permissions:
   
     Example permission policy for the Lambda role:
   
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "sqs:ReceiveMessage",
               "Resource": "arn:aws:sqs:region:account-id:MyQueue"
           },
           {
               "Effect": "Allow",
               "Action": "sqs:DeleteMessage",
               "Resource": "arn:aws:sqs:region:account-id:MyQueue"
           },
           {
               "Effect": "Allow",
               "Action": "sqs:GetQueueAttributes",
               "Resource": "arn:aws:sqs:region:account-id:MyQueue"
           }
       ]
   }
   ```

4. **Send a Test Message to the SQS Queue**:
   - Go to the **SQS** service in AWS.
   - Select your queue (`MyQueue`).
   - Click on **Send and receive messages**.
   - Enter a message body (e.g., `"Test message for Lambda"`).
   - Click **Send message**.

---

## 3. **Test the Lambda Function Invocation**

After setting up the SQS trigger, the Lambda function will be invoked automatically when a message is sent to the SQS queue.

### Steps to Test the Lambda Function:
1. **Send a Test Message**:
   - Go to the **SQS** console and send a test message to the queue as described earlier.
   
2. **Monitor the Lambda Execution**:
   - Go to the **Lambda** console and check the **Monitoring** tab.
   - View the CloudWatch logs to confirm that the Lambda function has been triggered and that the message was processed.

3. **Check CloudWatch Logs**:
   - In the **CloudWatch Logs** console, search for the log group corresponding to your Lambda function.
   - You should see logs similar to this:

   ```
   Received event: 
   {
       "Records": [
           {
               "messageId": "xxxxxx",
               "receiptHandle": "xxxxxx",
               "body": "Test message for Lambda",
               "attributes": {...},
               "messageAttributes": {},
               "md5OfBody": "xxxxxx",
               "eventSource": "aws:sqs",
               "eventSourceARN": "arn:aws:sqs:region:account-id:MyQueue",
               "awsRegion": "us-west-2"
           }
       ]
   }
   Processing message: Test message for Lambda
   ```

---

## Conclusion

In this solution, we've created an AWS Lambda function that is triggered by an SQS queue. When a message is sent to the queue, the Lambda function is automatically invoked to process it. This architecture helps XYZ Corporation to implement a serverless solution where resources are used only when needed, and no servers are running all the time, thus optimizing costs.
