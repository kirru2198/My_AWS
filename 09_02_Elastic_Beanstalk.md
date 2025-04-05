# Elastic Beanstalk

## Problem Statement:
You work for **XYZ Corporation**. Your corporation wants to launch a new web-based application and they do not want their servers to be running all the time. It should also be managed by AWS. Implement suitable solutions.

## Tasks To Be Performed:
1. **Create an Elastic Beanstalk environment** with the runtime set to **PHP**.
2. **Upload a simple PHP file** to the environment once created.

---
# Solution: Deploying a PHP Application with AWS Elastic Beanstalk

To meet the requirements of XYZ Corporation, we will use **AWS Elastic Beanstalk** to manage the web-based application. Elastic Beanstalk provides a managed environment for running applications, automatically handling tasks like scaling, load balancing, and monitoring, so you don’t have to manage the infrastructure. 

## 1. **Create an Elastic Beanstalk Environment with PHP Runtime**

### Steps to Create an Elastic Beanstalk Environment:
1. **Sign in to AWS Management Console**:
   - Go to the **AWS Management Console** and navigate to **Elastic Beanstalk**.

2. **Create a New Application**:
   - Click on **Create New Application**.
   - Enter a name for the application (e.g., `MyPHPApp`).
   - Optionally, provide a description for the application.

3. **Create an Environment**:
   - Click on **Create environment** under the application.
   - Choose **Web server environment** for the environment tier.
   
4. **Configure Platform**:
   - Select **PHP** as the platform for the environment.
   - AWS will automatically choose the appropriate version of PHP based on the latest supported versions.
   
5. **Environment Configuration**:
   - Set the **Environment name** (e.g., `my-php-environment`).
   - Optionally, you can choose to use **Load balancing** or **single-instance** based on your needs (for simplicity, choose single-instance for now).
   - You can configure other settings like instance type, scaling, and VPC if needed, but the default settings work for most cases.
   
6. **Review and Create**:
   - Review the configuration and click **Create environment**.
   - AWS will start provisioning resources for your application, and the environment will be created within a few minutes.
   
7. **Wait for Deployment**:
   - Once the environment is created, Elastic Beanstalk will deploy your application automatically.

---

## 2. **Upload a Simple PHP File to the Environment**

### Steps to Upload and Deploy a Simple PHP File:
1. **Create the PHP File**:
   - On your local machine, create a simple PHP file (e.g., `index.php`).
   
   Example of a simple `index.php` file:
   ```php
   <?php
   echo "Hello, World! Welcome to my PHP application on Elastic Beanstalk!";
   ?>
   ```

2. **Package the PHP File**:
   - Create a **.zip** file containing your `index.php`. Ensure that the file is directly inside the zip (not inside a folder) so Elastic Beanstalk can access it correctly.
   
3. **Upload the PHP File**:
   - Go to your **Elastic Beanstalk environment** dashboard.
   - Click on the **Upload and Deploy** button under the **Actions** section.
   - Choose the `.zip` file you created and click **Deploy**.
   
4. **Wait for Deployment**:
   - Elastic Beanstalk will now upload the PHP file and deploy it to the environment. This may take a few minutes.
   
5. **Access the Application**:
   - After the deployment is successful, go to the environment URL provided by Elastic Beanstalk (you can find this in the environment dashboard).
   - Open the URL in your browser, and you should see the message from your `index.php` file:  
   `"Hello, World! Welcome to my PHP application on Elastic Beanstalk!"`.

---

## Conclusion

You have successfully created an **Elastic Beanstalk environment** with a **PHP runtime** and uploaded a simple PHP application. Elastic Beanstalk automatically manages the environment for you, including scaling, monitoring, and updating. This solution meets the requirement of running a web-based application without the need for manually managing servers.
