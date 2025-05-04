 Problem Statement:
 You work for XYZ Corporation that uses on premise solutions and some limited number of systems. With the increase in requests in their application, the load also increases. So, to handle the load the corporation has to buy more systems almost on a regular basis. Realizing the need to cut down the expenses on systems, they decided to move their infrastructure to AWS.
 
 Tasks To Be Performed:
 1. Create a Classic Load Balancer and register 3 EC2 instances with different web pages running in them.
 2. Migrate the Classic Load Balancer into an Application Load Balancer.

 ---
To address the problem statement where XYZ Corporation wants to move its infrastructure to AWS and handle increased load using a load balancing solution, here are the detailed steps to achieve the tasks:

### **Task 1: Create a Classic Load Balancer and Register 3 EC2 Instances with Different Web Pages**

#### **Step 1: Launch 3 EC2 Instances with Different Web Pages**

1. **Log in to AWS Management Console**.

2. Navigate to **EC2** from the Services menu.

3. Click **Launch Instance** to launch 3 EC2 instances.

   - Choose a suitable **Amazon Machine Image (AMI)**, such as **Amazon Linux 2 AMI** or **Ubuntu Server**.
   - Select an instance type (e.g., **t2.micro** for a test environment).
   - Set the **VPC**, **subnet**, and **security group** (ensure ports 80 and 22 are open for web traffic and SSH).
   - **Key Pair**: Choose or create a new key pair to SSH into the instances.

4. **Configure the Web Servers**:

   - SSH into each EC2 instance (using the key pair you created earlier).
   - Install a simple web server (Apache, Nginx, or similar).

   For **Amazon Linux 2**:

   ```bash
   sudo yum update -y
   sudo yum install -y httpd
   sudo service httpd start
   sudo chkconfig httpd on
   echo "<h1>This is Web Server 1</h1>" | sudo tee /var/www/html/index.html
   ```

   For other instances, repeat the above steps with different content in the `index.html` file (e.g., "Web Page 2" for the second instance and "Web Page 3" for the third one).

5. **Confirm that the web pages are accessible**:

   - From your browser, access each EC2 instance’s public IP address. Each instance should display a different web page.

#### **Step 2: Create a Classic Load Balancer (ELB)**

1. **Navigate to the EC2 Dashboard** and click on **Load Balancers** under the **Load Balancing** section.

2. Click on **Create Load Balancer**.

3. **Choose Classic Load Balancer**:

   - Select **Create** under the **Classic Load Balancer** section.

4. **Configure the Load Balancer**:

   - **Name**: Give the load balancer a name, such as `MyClassicLoadBalancer`.
   - **Scheme**: Choose **Internet-facing** (if you want it accessible over the internet).
   - **Listeners**: Leave it as the default **HTTP** (Port 80).
   - **VPC**: Select the VPC where the EC2 instances are located.

5. **Configure Security Settings**:

   - If you're using HTTP (Port 80), there is no need for an SSL certificate. If using HTTPS, you would need to add a certificate, but for this task, **HTTP** suffices.

6. **Configure Security Groups**:

   - Select an existing security group or create a new one that allows inbound HTTP (port 80) traffic.

7. **Configure Health Checks**:

   - For health checks, set the **Ping Protocol** to HTTP, and **Ping Port** to `80`. For **Path**, use `/` (default).
   - Set the **Response Timeout** and **Interval** to defaults. Set the **Unhealthy threshold** to 2 and **Healthy threshold** to 2.

8. **Add EC2 Instances**:

   - After configuring the health checks, click **Next: Add EC2 Instances**.
   - Select the EC2 instances you launched earlier (ensure all three EC2 instances are in a running state).
   - Click **Next: Add Tags**.

9. **Review and Create**:

   - Review the configuration and click **Create**. The load balancer will be created, and once ready, it will distribute traffic among the EC2 instances.

   **Test the Load Balancer**:

   - Find the **DNS name** of the load balancer (it will look like `myclassicloadbalancer-xxxx.elb.amazonaws.com`).
   - Open your browser and access the load balancer’s DNS name. You should see the content from one of the EC2 instances. Refreshing the page should load content from the different instances based on load balancing.

---

### **Task 2: Migrate the Classic Load Balancer into an Application Load Balancer**

#### **Step 1: Create an Application Load Balancer (ALB)**

1. **Navigate to EC2 Dashboard** and click on **Load Balancers** again under the **Load Balancing** section.

2. Click **Create Load Balancer**, but this time choose **Application Load Balancer** (ALB).

3. **Configure the Application Load Balancer**:

   - **Name**: Provide a name for the new load balancer, e.g., `MyApplicationLoadBalancer`.
   - **Scheme**: Select **Internet-facing**.
   - **Listeners**: Ensure the **HTTP** listener (port 80) is selected.
   - **VPC**: Choose the VPC where your EC2 instances are deployed.
   - **Availability Zones**: Select the availability zones that your EC2 instances are in.

4. **Configure Security Settings**:

   - No SSL is required for HTTP. You can add SSL in future if required.

5. **Configure Security Groups**:

   - Choose a security group that allows HTTP traffic (port 80) to the load balancer.

6. **Configure Routing**:

   - **Target Group Name**: Give the target group a name like `MyTargetGroup`.
   - **Target type**: Choose **Instance** as the target type.
   - **Protocol**: Select **HTTP**.
   - **Port**: Set to **80**.
   - **Health Checks**: Set **Path** to `/` for HTTP health checks.

7. **Register Targets (EC2 Instances)**:

   - Click on **Next** to register the EC2 instances (same instances as in the Classic Load Balancer).
   - Select the instances and click **Include as targets**.

8. **Review and Create**:

   - Review the configuration and click **Create**.

#### **Step 2: Test the Application Load Balancer (ALB)**

1. **DNS Name**: After the ALB is created, note down its **DNS name** (it will look like `myapplicationloadbalancer-xxxx.elb.amazonaws.com`).

2. **Test the Load Balancer**:

   - Access the ALB's DNS name in your browser. The traffic should be distributed among the three EC2 instances.
   - You should see the different web pages hosted on each instance.

---

#### **Step 3: Decommission the Classic Load Balancer**

1. Once the Application Load Balancer is tested and working, you can **deregister** the EC2 instances from the Classic Load Balancer (if you wish to fully migrate).
2. Go to the **Classic Load Balancer**, and **deregister the instances** that you have moved to the ALB.
3. Optionally, you can **delete** the Classic Load Balancer if you no longer need it.

---

### **Summary of Tasks Completed:**

| Task                                                                                        | Status      |
| ------------------------------------------------------------------------------------------- | ----------- |
| Created a **Classic Load Balancer** and registered 3 EC2 instances with different web pages | ✅ Completed |
| Migrated the Classic Load Balancer to an **Application Load Balancer**                      | ✅ Completed |

---

By following these steps, XYZ Corporation can now handle the increased load efficiently using a load balancer and migrate smoothly from a Classic Load Balancer to an Application Load Balancer.

 
