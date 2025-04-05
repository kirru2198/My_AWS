# Project-1: Deploying a Multi-Tier Website Using AWS EC2

## Description:
Amazon Elastic Compute Cloud (Amazon EC2) provides scalable computing capacity in the Amazon Web Services (AWS) cloud. Using Amazon EC2 eliminates your need to invest in hardware up front so you can develop and deploy applications faster. You can use Amazon EC2 to launch as many or as few virtual servers as you need, configure security and networking, and manage storage. Amazon EC2 enables you to scale up or down to handle changes in requirements or spikes in popularity, reducing your need to forecast traffic.

## Problem Statement:
Company ABC wants to move their product to AWS. They have the following things set up right now:
1. MySQL DB
2. Website (PHP)

The company wants high availability for this product and therefore wants Auto Scaling to be enabled on this website.

## Steps to Solve:
1. **Launch an EC2 Instance**
2. **Enable Auto Scaling on these instances** (minimum 2)
3. **Create an RDS Instance**
4. **Create Database & Table in RDS instance**:
   - **Database name**: intel
   - **Table name**: data
   - **Database password**: intel123
5. **Change hostname in the website**
6. **Allow traffic from EC2 to RDS instance**
7. **Allow all traffic to EC2 instance**
   
---

## **Solution to Project-1: Deploying a Multi-Tier Website Using AWS EC2**

### **1. Launch an EC2 Instance**

To launch an EC2 instance:

1. **Login to AWS Management Console**:
   - Go to [AWS Console](https://aws.amazon.com/console/) and log in with your credentials.

2. **Navigate to EC2**:
   - In the search bar, type **EC2** and click on **EC2** under Services.

3. **Launch Instance**:
   - Click on **Launch Instance** to create a new EC2 instance.
   - Choose an **Amazon Machine Image (AMI)**. For PHP, you can use **Amazon Linux 2** or **Ubuntu**.
   - Choose an **Instance Type**. You can start with a small instance like `t2.micro` for testing or a larger instance depending on the expected load.
   - **Configure Instance**: Set the number of instances to **1** for now (we will enable auto-scaling in the next step).
   - **Add Storage**: Configure the storage settings (default is usually sufficient for testing purposes).
   - **Configure Security Group**: Create a new security group or select an existing one to allow HTTP (port 80) and SSH (port 22) traffic to the instance.
   - **Review and Launch**: Review your settings, select your key pair, and click **Launch** to start the instance.

### **2. Enable Auto Scaling on these Instances (Minimum 2)**

To enable Auto Scaling for your website:

1. **Navigate to Auto Scaling**:
   - In the AWS Console, go to the **EC2 Dashboard**, then click on **Auto Scaling**.

2. **Create an Auto Scaling Group**:
   - Click **Create Auto Scaling group**.
   - Choose the launch configuration (either create a new one or use an existing one that defines your EC2 instance).
   - Set the **Desired Capacity** to 2 instances, which means 2 EC2 instances will always run.
   - **Configure Scaling Policies**: Set up scaling policies such as:
     - **Scale Out** (increase the number of instances) if CPU usage is high.
     - **Scale In** (decrease the number of instances) if CPU usage is low.
   - Define the **VPC**, **subnet**, and **load balancer** (if needed).
   - Click **Create Auto Scaling Group**.

### **3. Create an RDS Instance**

To create an RDS instance for MySQL:

1. **Navigate to RDS**:
   - In the AWS Management Console, search for and select **RDS** under Services.

2. **Create Database**:
   - Click **Create database** and choose the **Standard Create** option.
   - Select **MariaDB** or **MySQL** as the database engine (since you have MySQL).
   - Choose the **Free Tier** if you want to use a free tier database or select the appropriate option based on your needs.

3. **Configure Database Settings**:
   - **DB Instance Identifier**: Name your database instance (e.g., `intel-db`).
   - **Master Username**: Set the username (e.g., `admin`).
   - **Master Password**: Set a strong password (e.g., `intel123`).
   - Choose the appropriate **DB Instance Class**, **Storage Type**, and **Backup settings**.
   - Ensure the **Public Accessibility** is set to **Yes** if you want the instance to be accessible from outside the VPC (or set it to **No** for internal access).

4. **Launch RDS Instance**:
   - Once configured, click **Create database** to launch the RDS instance.

### **4. Create Database & Table in RDS Instance**

Once the RDS instance is up and running, you can connect to it and create the required database and table.

1. **Connect to the RDS Instance**:
   - Find the **endpoint** of the RDS instance from the RDS dashboard.
   - You can use a MySQL client (like MySQL Workbench or CLI) to connect:
     ```bash
     mysql -h <rds-endpoint> -u admin -p
     ```
   - Enter the master password (`intel123`).

2. **Create the Database**:
   - After logging in, run the following SQL commands:
     ```sql
     CREATE DATABASE intel;
     USE intel;
     ```

3. **Create the Table**:
   - Run the following SQL command to create the required table:
     ```sql
     CREATE TABLE data (
         id INT AUTO_INCREMENT PRIMARY KEY,
         name VARCHAR(255),
         description TEXT
     );
     ```

### **5. Change Hostname in Website**

To connect your website to the newly created RDS database:

1. **Locate Database Connection in Website**:
   - In your PHP website, locate the section where the database connection is configured (usually in a `config.php` or similar file).

2. **Update Hostname**:
   - Change the hostname to point to the RDS instance:
     ```php
     $servername = "<rds-endpoint>";
     $username = "admin";
     $password = "intel123";
     $dbname = "intel";
     ```
   - Update the connection parameters as necessary.

### **6. Allow Traffic from EC2 to RDS Instance**

To allow traffic from the EC2 instances to the RDS instance:

1. **Update RDS Security Group**:
   - Go to the **RDS Dashboard** and find the security group attached to your RDS instance.
   - Modify the **Inbound Rules** to allow traffic on port **3306** (MySQL port) from the EC2 instances’ security group.

2. **Update EC2 Security Group**:
   - Ensure that the security group attached to your EC2 instances allows outbound traffic to the RDS instance on port 3306.

### **7. Allow All Traffic to EC2 Instance**

To allow all traffic to the EC2 instance (optional for testing purposes):

1. **Update EC2 Security Group**:
   - Go to the **EC2 Dashboard** and find the security group attached to your EC2 instance.
   - Add an **Inbound Rule** to allow all traffic (`0.0.0.0/0` for IPv4 or `::/0` for IPv6) on ports 22 (SSH), 80 (HTTP), and 443 (HTTPS).

   **Note**: It's important to limit access to necessary IP ranges for security in a production environment.

---

### **Summary of Steps:**
1. Launch an EC2 instance and configure auto-scaling with a minimum of 2 instances.
2. Create a MySQL-based RDS instance and configure the database (`intel`) and table (`data`).
3. Update your PHP website's database connection to use the RDS instance.
4. Allow traffic from EC2 to RDS, and ensure your EC2 instance is accessible from the internet for testing.

---

This solution should guide you through the process of deploying a high-availability multi-tier website using AWS EC2 and RDS. Let me know if you need any further clarification or assistance!
 
