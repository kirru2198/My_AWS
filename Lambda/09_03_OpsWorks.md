# OpsWorks

## Problem Statement:
You work for **XYZ Corporation**. Your corporation wants to launch a new web-based application and they do not want their servers to be running all the time. It should also be managed by AWS. Implement suitable solutions.

## Tasks To Be Performed:
1. **Create an OpsWorks sample stack**, start the instances, and deploy the application.
2. **Add 2 more t2.medium instances**.
3. **Make a change to the repository code** and check if it reflects in all the instances.

---
# Solution: Managing a Web Application with AWS OpsWorks

AWS **OpsWorks** is a configuration management service that provides a fully managed service for running and scaling applications. It is ideal for managing environments where you need flexibility but do not want to manage your infrastructure continuously. Below is a step-by-step solution to implement the requirements.

## 1. **Create an OpsWorks Sample Stack, Start the Instances, and Deploy the Application**

### Steps to Create an OpsWorks Stack:
1. **Sign in to AWS Management Console**:
   - Navigate to **OpsWorks** in the AWS Management Console.

2. **Create a New Stack**:
   - Click on **Create stack**.
   - Choose **Chef** or **Puppet** as the configuration management type (Chef is commonly used).
   - Provide a **Stack name** (e.g., `MyWebAppStack`).
   - Choose the **Region** where you want to launch the stack.

3. **Configure the Stack**:
   - Under **Stack settings**, you can configure general stack parameters like **VPC** settings, **SSH key pairs** for access, and **IAM role**. For a basic setup, you can leave the default values.
   
4. **Create a Layer for the Application**:
   - Click on **Add Layer**.
   - Select the **Custom layer** or a **PHP layer** depending on the application stack you want to deploy.
   - Provide a **Layer name** (e.g., `WebLayer`).
   - Specify the **short name** (e.g., `web`).
   - For the **Instance type**, select `t2.micro` or any other instance type you prefer.
   - Select **Auto scaling** options if needed.
   
5. **Add Instances to the Layer**:
   - Click on **Add Instance** to add an instance to the stack.
   - Configure the instance settings, such as **instance type**, **hostname**, **operating system** (e.g., Amazon Linux), and **availability zone**.
   - Click **Add** to create the instance.
   
6. **Deploy the Application**:
   - To deploy your application, you can either use **Chef** or **Puppet** to automate the deployment process or manually upload the application code.
   - In the **OpsWorks dashboard**, navigate to the **Layers** section, select your **WebLayer**, and click on **Deploy** to push the application to the instance.

### Confirm Deployment:
- After the instance is launched, navigate to the public IP of your instance to confirm that the application is successfully deployed.

---

## 2. **Add 2 More t2.medium Instances**

### Steps to Add More Instances:
1. **Navigate to Your Stack in OpsWorks**:
   - In the AWS Management Console, go to **OpsWorks** and open the stack you created in step 1.

2. **Add New Instances**:
   - Click on the **Instances** tab within your stack.
   - Click on **Add instance** to add a new instance to the layer.
   - Select **t2.medium** for the instance type.
   - Configure the instance parameters such as **hostname**, **instance size**, and **availability zone**.
   - Make sure to select the layer you want the instance to belong to (e.g., `WebLayer`).
   
3. **Launch the New Instances**:
   - Once the instances are added, click **Add** to launch them.
   - Wait for the instances to start and become healthy in the OpsWorks dashboard.

4. **Verify Instances**:
   - Once the instances are launched and running, navigate to the **Instances** tab in OpsWorks, and verify that all 3 instances (1 initial and 2 new t2.medium instances) are running and healthy.

---

## 3. **Make a Change to the Repository Code and Check if It Reflects in All the Instances**

### Steps to Make a Change in the Code and Reflect it Across Instances:
1. **Access the Code Repository**:
   - OpsWorks supports integration with **Git repositories** for code management. If you have already linked a repository to your stack, you can update the code from there.
   
2. **Make the Code Change**:
   - Clone your repository to your local machine if it's not already done.
   - Make the necessary code changes in your local development environment.
   - Push the changes back to the Git repository.

3. **Deploy the Changes to the Stack**:
   - Go back to the **OpsWorks** console.
   - Navigate to the **Layers** tab and select the layer where your application is deployed.
   - Click on **Deploy** to deploy the updated code to all the instances in the layer.
   - If you're using **Chef** or **Puppet** for automation, ensure that the deployment scripts are configured to pull the latest code changes from the repository.

4. **Verify the Code Changes Across Instances**:
   - After the deployment finishes, verify that the new code changes are reflected on all instances by checking the public IPs or the URLs of the instances.
   - All instances should now serve the updated application code.

---

## Conclusion

Using **AWS OpsWorks**, you can efficiently manage and scale your web application environment. Here's a summary of the steps you completed:

1. Created an **OpsWorks stack** and deployed a sample web application.
2. Added **2 additional t2.medium instances** to the stack for scaling.
3. Made **code changes** to the repository and ensured that the changes reflected across all the instances in the stack.

This solution provides a flexible, scalable, and managed approach to running your application in the cloud without the need for managing servers manually.
