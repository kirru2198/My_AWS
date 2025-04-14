# Case Study

## Problem Statement:
You work for XYZ organization. Your organization uses AWS cloud to host their web applications.

## Tasks To Be Performed:

1. **Create a Beanstalk environment to host a Custom PHP application**  
   Create a Beanstalk environment in AWS to host a custom PHP application.

2. **Upload a new PHP file which reads “Updated Hello World” to the Beanstalk environment**  
   Upload a PHP file to the Beanstalk environment that outputs the message "Updated Hello World".

3. **Create a Lambda function which uses S3 bucket object creation as a trigger**  
   Create an AWS Lambda function that is triggered by object creation events in an S3 bucket.

4. **Launch an OpsWorks stack and host a Hello World HTML page in the instance of that stack**  
   Launch an AWS OpsWorks stack and host a simple "Hello World" HTML page within an instance in that stack.

---
Certainly! Below is a detailed solution to the tasks outlined in the case study.

```markdown
# Solution to Case Study

## 1. Create a Beanstalk Environment to Host a Custom PHP Application

### Steps:
1. **Log into AWS Management Console**: Go to the [AWS Console](https://aws.amazon.com/console/).
2. **Navigate to Elastic Beanstalk**:
   - Search for "Elastic Beanstalk" in the AWS services search bar and select it.
3. **Create a New Application**:
   - Click on the "Create a new application" button.
   - Provide an application name (e.g., `MyPHPApp`).
4. **Create Environment**:
   - Select "Web server environment" for the environment tier.
   - For the platform, choose "PHP".
   - Configure other environment settings as needed (e.g., instance type, scaling, etc.).
5. **Upload the PHP Code**:
   - Create a ZIP file of your PHP application, including the `index.php` or any other PHP files.
   - Click "Upload and Deploy" to upload your ZIP file and deploy it to the environment.
6. **Review & Launch**:
   - Review your settings and click "Create environment" to launch the Beanstalk environment.
   
Elastic Beanstalk will automatically provision all the resources, including EC2, load balancers, and other configurations.

---

## 2. Upload a New PHP File to the Beanstalk Environment

### Steps:
1. **Prepare PHP File**: Create a new PHP file (e.g., `hello.php`) that reads:
   ```php
   <?php
   echo "Updated Hello World";
   ?>
   ```
2. **Deploy the PHP File**:
   - ZIP the PHP file and any other required files (e.g., configuration files).
   - Go to the Elastic Beanstalk console, select the environment where your app is hosted.
   - Click on "Upload and Deploy" and upload the new ZIP file.
   - Once the application is updated, navigate to the URL of your Beanstalk environment (e.g., `http://myphpapp.elasticbeanstalk.com/hello.php`) to verify the output, which should read "Updated Hello World".

---

## 3. Create a Lambda Function Triggered by S3 Bucket Object Creation

### Steps:
1. **Create an S3 Bucket**:
   - Navigate to S3 in the AWS Console.
   - Create a new S3 bucket to store your files (e.g., `my-bucket-name`).
   
2. **Create a Lambda Function**:
   - Navigate to Lambda in the AWS Console.
   - Click "Create function".
   - Select "Author from scratch".
   - Give your Lambda function a name (e.g., `S3ObjectCreationTrigger`).
   - Choose a runtime (e.g., Python or Node.js).
   - Set the role permissions (ensure the Lambda function has permission to access the S3 bucket).
   
3. **Set S3 Trigger**:
   - In the Lambda function's configuration page, click "Add trigger".
   - Select "S3" as the trigger.
   - Choose the bucket (`my-bucket-name`) and select "All object create events".
   - Save the trigger.
   
4. **Lambda Code**:
   - For this example, here's a simple Python code for Lambda that prints the S3 event data:
   ```python
   import json

   def lambda_handler(event, context):
       # Print event data
       print("Event data received:", json.dumps(event))
       return {
           'statusCode': 200,
           'body': json.dumps('Successfully processed the event!')
       }
   ```
   
5. **Test the Lambda Function**:
   - Upload a file to the S3 bucket.
   - Verify in the Lambda function's logs that it was triggered by the S3 event.

---

## 4. Launch an OpsWorks Stack and Host a "Hello World" HTML Page

### Steps:
1. **Create an OpsWorks Stack**:
   - Navigate to AWS OpsWorks in the AWS Console.
   - Click on "Create stack".
   - Choose a stack name (e.g., `MyOpsWorksStack`).
   - Select the region, and choose your preferred settings for the stack.
   
2. **Create a Layer**:
   - After creating the stack, click "Add layer".
   - Choose a layer type (e.g., `Custom Layer` for basic HTTP server).
   - Provide the necessary configuration (e.g., instance size, operating system).

3. **Create an Instance**:
   - After creating the layer, click "Add instance" and configure the instance settings (e.g., instance type, SSH key).
   - Launch the instance within the layer.

4. **Deploy a "Hello World" HTML Page**:
   - SSH into the OpsWorks instance using the key pair created earlier.
   - Navigate to the web server's root directory (usually `/var/www/html`).
   - Create a new `index.html` file with the following content:
   ```html
   <html>
     <head><title>Hello World</title></head>
     <body>
       <h1>Hello World from AWS OpsWorks</h1>
     </body>
   </html>
   ```
   
5. **Verify**:
   - Open the public IP of the instance in your browser (e.g., `http://<public-ip>`) to confirm the page is showing the "Hello World" message.

---

This solution covers all the necessary steps for each task in the case study.
``` 

This markdown format solution explains each step clearly to accomplish the tasks using AWS services.
