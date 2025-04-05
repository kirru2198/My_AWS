# Website Orchestration

## Industry: Internet Related

### Problem Statement:
How to orchestrate a website with lesser time and higher availability along with Auto Scaling.

### Topics:
In this AWS project, you will deploy a high-availability PHP application with an external Amazon RDS database to Elastic Beanstalk. Running a DB instance external to Elastic Beanstalk decouples the database from the lifecycle of your environment. This allows you to connect to the same database from multiple environments, swap one database for another, or perform a blue/green deployment without affecting your database.

### Highlights:
- Launch a DB instance in **Amazon RDS**.
- Create an **Elastic Beanstalk Environment**.
- Configure **Security Groups** and **Scaling**.

---
# Solution: Website Orchestration with AWS

## 1. **Launch a DB Instance in Amazon RDS**

The first step in our solution is to create a highly available database instance using **Amazon RDS** (Relational Database Service). This external database will be decoupled from the Elastic Beanstalk environment.

### Steps to Launch RDS DB Instance:
1. **Sign in to AWS Management Console** and navigate to **RDS**.
2. **Create a new DB Instance**:
   - Select the DB engine (e.g., MySQL, PostgreSQL).
   - Choose a DB instance class based on your expected load.
   - Set the database identifier (e.g., `mydb-instance`).
   - Choose **Multi-AZ deployment** for high availability.
   - Set up **VPC**, **subnet**, and **security group** settings for network access.
   - Configure **backup retention**, **maintenance windows**, and **monitoring** as needed.
3. **Database Credentials**:
   - Set the master username and password for the database.
4. **Security**:
   - Ensure the RDS instance is associated with the correct security group that allows inbound traffic from Elastic Beanstalk instances.
5. **Launch the DB Instance**:
   - Review the settings and launch the instance.
6. **Obtain Endpoint Information**:
   - After the DB instance is available, note the **endpoint** and **port** for connecting your application later.

---

## 2. **Create an Elastic Beanstalk Environment**

Next, we will create an Elastic Beanstalk environment to deploy the PHP application. This environment will connect to the external Amazon RDS database.

### Steps to Create Elastic Beanstalk Environment:
1. **Sign in to AWS Management Console** and navigate to **Elastic Beanstalk**.
2. **Create a New Application**:
   - Select **Create New Application** and give it a name (e.g., `MyPHPApp`).
   - Choose **PHP** as the platform.
3. **Deploy the PHP Application**:
   - Upload the PHP application code (e.g., `.zip` file).
   - Set up the necessary environment variables, such as the RDS database credentials (`DB_HOST`, `DB_USER`, `DB_PASSWORD`, `DB_NAME`).
4. **Environment Configuration**:
   - Choose a **single-instance** or **load balanced** environment (to scale horizontally).
   - Configure the **instance type** (e.g., `t2.micro` for testing or larger for production).
5. **Select the VPC**:
   - Choose the VPC that has the RDS instance and ensure Elastic Beanstalk instances can communicate with RDS.
6. **Create the Environment**:
   - Review and create the environment.

---

## 3. **Configure Security Groups and Scaling**

To ensure high availability and security, we need to configure security groups and auto-scaling.

### Steps to Configure Security Groups:
1. **Security Group for RDS**:
   - The RDS instance needs to be in a security group that allows inbound connections from the Elastic Beanstalk instances.
   - Modify the RDS security group to allow inbound traffic on the database port (e.g., 3306 for MySQL) from the **Elastic Beanstalk security group**.
   
2. **Security Group for Elastic Beanstalk**:
   - Elastic Beanstalk creates a default security group for your environment, but you may want to add custom rules.
   - Ensure the security group allows HTTP/HTTPS traffic (ports 80/443) for external access and database traffic (e.g., 3306 for MySQL) for internal communication with RDS.

### Steps to Configure Auto Scaling:
1. **Auto Scaling for Elastic Beanstalk**:
   - Elastic Beanstalk automatically creates an Auto Scaling group to manage instance scaling based on demand.
   - You can configure the scaling settings under the **Elastic Beanstalk environment configuration**:
     - Set the **minimum** and **maximum number of instances** based on expected traffic.
     - Choose a **scaling policy** to adjust the number of running instances based on CPU utilization, network traffic, or other metrics.
   - Elastic Load Balancer (ELB) will be configured automatically for load balancing between instances in a load-balanced environment.

2. **Configure RDS Read Replicas** (Optional):
   - For high availability, configure **Read Replicas** for your RDS instance. This helps offload read traffic and increase availability.
   - RDS Read Replicas can be added via the AWS RDS management console, and once they are configured, the application can be modified to route read queries to the replicas.

---

## 4. **Testing and Deployment**

### Steps to Test the Application:
1. **Access the Application**:
   - Once the Elastic Beanstalk environment is live, the application should be accessible via the URL provided by Elastic Beanstalk.
2. **Test Database Connectivity**:
   - Ensure that the PHP application is properly connecting to the RDS database using the connection strings provided in the environment variables.
   - Perform CRUD operations (Create, Read, Update, Delete) to ensure that the application communicates with the database as expected.
3. **Monitor the Environment**:
   - Use **CloudWatch** to monitor the health of both Elastic Beanstalk and RDS instances.
   - Set up alarms to notify if the application or database reaches a critical threshold.

---

## 5. **Blue/Green Deployment (Optional)**

To perform **blue/green deployments** without affecting the database:
1. **Create a New Elastic Beanstalk Environment** (Green environment) while maintaining the current environment (Blue).
2. **Deploy the New Version** of the PHP application to the Green environment.
3. **Swap CNAME** to point to the new environment after successful testing.

This ensures minimal downtime and smooth transition between application versions.

---

### Conclusion

By using Amazon RDS for the database and Elastic Beanstalk for the application, we have achieved a highly available and scalable architecture. With proper security configurations, scaling policies, and the option for blue/green deployments, the solution provides both flexibility and reliability for managing a website in AWS.
