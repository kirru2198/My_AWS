# Assignment-01

## Problem Statement:
You work for XYZ Corporation. Their application requires a SQL service that can store data which can be retrieved when needed. Implement a suitable RDS engine for the same.

While migrating, you are asked to perform the following tasks:

### Tasks:
1. Create a MariaDB Engine based RDS Database.
2. Connect to the Database using the following methods:
   - a. SQL Client for Windows
   - b. Linux based EC2 Instance

---
Below is a detailed solution to the problem statement provided, including steps for setting up a MariaDB Engine-based RDS database and how to connect to it using a Windows SQL client and a Linux-based EC2 instance.

---

## **Solution to Assignment-01**

### **1. Create a MariaDB Engine-based RDS Database:**

To create a MariaDB-based RDS database, follow these steps in the AWS Management Console:

#### **Step-by-Step Process:**

1. **Login to AWS Management Console**:
   - Go to the AWS Management Console and log in with your credentials.

2. **Navigate to Amazon RDS**:
   - In the search bar, type **RDS** and click on **RDS** under Services.

3. **Launch a New RDS Instance**:
   - On the RDS dashboard, click on **Create database**.
   
4. **Choose Database Engine**:
   - Select **MariaDB** from the list of available database engines.

5. **Choose Template**:
   - Select the **Free tier** template if you are working with the free-tier eligibility or choose **Production** for a larger setup.

6. **Specify Database Settings**:
   - **DB Instance Identifier**: Choose a unique name for your DB instance (e.g., `mariadb-db`).
   - **Master Username**: Set a master username (e.g., `admin`).
   - **Master Password**: Set a strong password and confirm it.
   - **DB Instance Class**: Choose an instance size (e.g., `db.t2.micro` for free tier).
   - **Storage Type**: Choose the storage type (e.g., General Purpose (SSD)).
   
7. **Configure Advanced Settings**:
   - **VPC**: Use the default VPC, or configure a custom VPC.
   - **Public Accessibility**: Set to **Yes** if you want to access the DB instance from outside the VPC.
   - **VPC Security Groups**: Choose or create a security group to control access to the DB instance.
   - **Backup & Monitoring**: Adjust these settings as per your needs, such as enabling automatic backups.

8. **Launch the RDS Instance**:
   - After configuring, click **Create database** to launch the instance.

---

### **2. Connect to the Database:**

After creating the MariaDB RDS instance, you can connect to it in two different ways:

#### **a. Connect Using SQL Client for Windows**

To connect using an SQL client such as **MySQL Workbench** or **DBeaver** on Windows:

1. **Install MySQL Workbench** (or any preferred SQL client) on your Windows machine from [MySQL Workbench Download](https://dev.mysql.com/downloads/workbench/).

2. **Find the Endpoint of the RDS Instance**:
   - Go to your RDS instance in the AWS Management Console.
   - Under the **Connectivity & security** tab, copy the **Endpoint** (e.g., `mariadb-db.xxxxxxx.us-east-1.rds.amazonaws.com`).

3. **Open MySQL Workbench**:
   - Open **MySQL Workbench** and click on **+** to add a new connection.

4. **Enter Connection Details**:
   - **Connection Name**: Any name you prefer.
   - **Connection Method**: Choose **Standard (TCP/IP)**.
   - **Hostname**: Paste the RDS endpoint you copied earlier.
   - **Port**: Default MariaDB port (3306).
   - **Username**: The master username you configured (e.g., `admin`).
   - **Password**: Click on **Store in Vault** and enter the master password.

5. **Test the Connection**:
   - Click on **Test Connection** to ensure everything is set up correctly.
   - If the test is successful, click **OK** to save the connection.

6. **Connect to the Database**:
   - Click on the newly created connection in **MySQL Workbench** to start querying the MariaDB RDS instance.

#### **b. Connect Using a Linux-based EC2 Instance**

To connect from an EC2 instance running Linux (e.g., Amazon Linux 2):

1. **Launch an EC2 Instance**:
   - If you don’t have an EC2 instance, create one using the Amazon EC2 Dashboard in the AWS Console.
   - Choose an **Amazon Linux 2** instance, and make sure the security group allows inbound SSH and MySQL connections (port 3306).

2. **SSH into the EC2 Instance**:
   - Use the following command to SSH into your EC2 instance:
     ```bash
     ssh -i /path/to/your-key.pem ec2-user@<ec2-public-ip>
     ```

3. **Install MariaDB Client on EC2**:
   - Install the MariaDB client on the EC2 instance using the following commands:
     ```bash
     sudo yum update -y
     sudo yum install mariadb -y
     ```

4. **Find the Endpoint of the RDS Instance**:
   - Go to your RDS instance in the AWS Management Console.
   - Under the **Connectivity & security** tab, copy the **Endpoint** of the MariaDB instance.

5. **Connect to the RDS Database**:
   - Use the MariaDB command line client to connect to your RDS instance:
     ```bash
     mysql -h <rds-endpoint> -u <master-username> -p
     ```
     Replace `<rds-endpoint>` with your RDS instance's endpoint, and `<master-username>` with the username you created.

   - Enter the master password when prompted.

6. **Verify the Connection**:
   - Once connected, you should be able to run SQL queries against your MariaDB instance.

---

### **Summary of Steps:**

1. Create a MariaDB RDS instance in the AWS Management Console.
2. Connect to the RDS instance using either a Windows-based SQL client or a Linux EC2 instance.
3. For SQL clients, you will need the RDS endpoint, the username, and password.
4. For EC2, you install the MariaDB client and connect using the same credentials.

---

This solution should help you implement the tasks as per the problem statement. Let me know if you need further clarification or assistance!

 
