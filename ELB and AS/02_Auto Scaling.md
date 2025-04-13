 Problem Statement:
 You work for XYZ Corporation that uses on premise solutions and some limited number of systems. With the increase in requests in their application, the load also increases. So, to handle the load the corporation has to buy more systems almost on a regular basis. Realizing the need to cut down the expenses on systems, they decided to move their infrastructure to AWS.
 
 Tasks To Be Performed:
 1. Create a web server AMI with Apache 2 server running in it.
 2. Create a launch configuration with this AMI.
 3. Use this launch configuration to create an Auto Scaling group with 1 minimum and 3 maximum instances.

---
To solve the problem where XYZ Corporation is looking to move its infrastructure to AWS, you need to create a web server AMI with Apache 2 running on it, create a launch configuration using this AMI, and then set up an Auto Scaling group with the desired instance configuration.

### **Task 1: Create a Web Server AMI with Apache 2 Running**

#### **Step 1: Launch an EC2 Instance**
1. **Login to AWS Console** and navigate to **EC2**.
2. Click on **Launch Instance**.
   - Choose an **Amazon Linux 2 AMI** (or another Linux-based AMI if preferred).
   - Select an **Instance Type** (e.g., `t2.micro` for testing).
   - Configure the instance details (choose the correct **VPC** and **Subnet**).
   - **Security Group**: Ensure HTTP (port 80) and SSH (port 22) are open.
   - Create or select a **Key Pair** to access the instance.

3. **Connect to the Instance** via SSH using the key pair.
   ```bash
   ssh -i your-key.pem ec2-user@<your-ec2-instance-ip>
   ```

4. **Install Apache Web Server**:
   Run the following commands to install and start the Apache web server:
   ```bash
   sudo yum update -y
   sudo yum install -y httpd
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

5. **Verify Apache is Working**:
   - Open a web browser and access your instance using its public IP address (e.g., `http://<your-ec2-instance-ip>`). You should see the default Apache welcome page.

6. **Create the Custom AMI**:
   - Go to **EC2 Dashboard** → **Instances**, and select your running instance.
   - Click on **Actions** → **Image and templates** → **Create Image**.
   - Provide a name for the image (e.g., `Apache-Web-Server-AMI`).
   - Click **Create Image**.
   
   AWS will create an AMI, which will be available in the **AMIs** section of the EC2 dashboard.

---

### **Task 2: Create a Launch Configuration with this AMI**

1. Navigate to the **EC2 Dashboard** and click on **Auto Scaling** under **AUTO SCALING** section.
2. Click on **Create launch configuration**.
3. **Choose the AMI**:
   - Select the **Apache-Web-Server-AMI** you created earlier from the AMI list.
   
4. **Choose Instance Type**:
   - Select an instance type (e.g., `t2.micro` for testing).
   
5. **Configure the Launch Configuration**:
   - **Key Pair**: Choose the same key pair you used for the EC2 instance (for SSH access).
   - **Security Groups**: Either choose an existing security group or create a new one with HTTP (port 80) and SSH (port 22) open.
   
6. **Advanced Details**: Leave these as default for now, unless specific configurations are needed.

7. **Create Launch Configuration**:
   - Review the settings and click **Create launch configuration**.

---

### **Task 3: Create an Auto Scaling Group with 1 Minimum and 3 Maximum Instances**

1. **Navigate to Auto Scaling**:
   - In the **EC2 Dashboard**, click on **Auto Scaling Groups** under the **AUTO SCALING** section.
   
2. **Create an Auto Scaling Group**:
   - Click on **Create Auto Scaling group**.
   - **Name**: Provide a name for the Auto Scaling group (e.g., `Web-Server-Scaling-Group`).
   
3. **Choose Launch Configuration**:
   - Select the **Launch Configuration** you just created (`Apache-Web-Server-Launch-Configuration`).
   
4. **Configure Auto Scaling Group Settings**:
   - **VPC**: Select the appropriate VPC and subnet(s).
   - **Availability Zones**: Choose the zones where you want the instances to be distributed.
   
5. **Set Scaling Policies**:
   - **Minimum Size**: Set this to **1** (minimum instance count).
   - **Maximum Size**: Set this to **3** (maximum instance count).
   - **Desired Capacity**: Set this to **1** (default desired number of instances when starting).
   
6. **Configure Health Checks**:
   - **Health Check Type**: Select **EC2** (or you can use ELB health check if using a load balancer).
   - **Health Check Grace Period**: Use the default value (300 seconds) or modify as per requirement.

7. **Configure Notifications (Optional)**:
   - You can set up notifications for scaling events (e.g., instances launching or terminating). This is optional.

8. **Review and Create**:
   - Review the settings and click **Create Auto Scaling Group**.

---

### **Testing and Monitoring the Auto Scaling Group**

1. **Test the Auto Scaling**:
   - To test if Auto Scaling works, go to the **Auto Scaling Group** and manually increase the desired capacity to 3. The Auto Scaling group should launch additional instances.
   - You can also simulate high load (e.g., via CPU load testing) to see if it scales up, and when load decreases, it should scale down.

2. **Monitor the Auto Scaling Group**:
   - You can monitor the scaling activity via the **CloudWatch** metrics or the **Auto Scaling Group** metrics to check the instance counts, CPU utilization, and other metrics.
   - **CloudWatch** will also show the scaling events when instances are added or removed.

---

### **Summary of Tasks Completed:**

| Task                                                    | Status       |
|---------------------------------------------------------|--------------|
| Created a **Web Server AMI** with Apache 2 running      | ✅ Completed |
| Created a **Launch Configuration** using the custom AMI | ✅ Completed |
| Set up an **Auto Scaling Group** with 1 minimum and 3 maximum instances | ✅ Completed |

---

This setup will help XYZ Corporation handle increased load dynamically by scaling the number of web server instances. 
