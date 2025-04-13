 Problem Statement:
 You work for XYZ Corporation that uses on premise solutions and some limited number of systems. With the increase in requests in their application, the load also increases. So, to handle the load the corporation has to buy more systems almost on a regular basis. Realizing the need to cut down the expenses on systems, they decided to move their infrastructure to AWS.
 
 Tasks To Be Performed:
 1. Use the Route 53 hosted zone created in the assignment.
 2. Route the traffic to an EC2 instance with an Apache web server running in it using its IP address.

 ---
 To solve the problem for XYZ Corporation, where they need to route traffic to an EC2 instance with an Apache web server using Route 53, here's a step-by-step guide:

### **Step 1: Use the Route 53 Hosted Zone**

Ensure that you have already created a **Route 53 hosted zone** for your domain, as per the previous assignment. If you haven't created it yet, follow these steps to create it:

1. **Log in to the AWS Management Console**.
2. **Navigate to Route 53** from the **Services** menu.
3. Under **DNS management**, select **Hosted zones**.
4. Click on **Create hosted zone** and fill in the following details:
   - **Domain Name**: Enter your domain (e.g., `xyzcorporation.com`).
   - **Type**: Choose **Public Hosted Zone**.
   - Leave the rest of the settings as default and click **Create**.

You should now have a Route 53 hosted zone for your domain.

---

### **Step 2: Set Up an EC2 Instance with Apache Web Server**

1. **Launch an EC2 Instance** with an Apache web server installed:
   - Go to the **EC2 Dashboard** in AWS and click **Launch Instance**.
   - Choose an appropriate **Amazon Machine Image (AMI)** (e.g., Amazon Linux 2 or Ubuntu).
   - Select an **instance type** (e.g., `t2.micro`).
   - Configure the **instance details**, **storage**, and **security groups** (ensure that HTTP port 80 is open in the security group for public access).
   - Use a **key pair** for SSH access to the EC2 instance.

2. **SSH into the EC2 Instance**:
   After the instance is running, you need to SSH into the instance to install and configure Apache.

   ```bash
   ssh -i your-key.pem ec2-user@<your-ec2-instance-public-ip>
   ```

3. **Install Apache Web Server**:
   Run the following commands to install Apache (if not already installed):
   ```bash
   sudo yum update -y  # For Amazon Linux
   sudo yum install -y httpd
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```

   Alternatively, for Ubuntu-based instances:
   ```bash
   sudo apt update -y
   sudo apt install -y apache2
   sudo systemctl start apache2
   sudo systemctl enable apache2
   ```

4. **Test Apache**:
   - Access the EC2 instance's **public IP address** in a web browser (e.g., `http://<your-ec2-instance-ip>`).
   - You should see the default Apache test page, confirming that the Apache server is running.

---

### **Step 3: Route Traffic Using Route 53**

1. **Obtain the EC2 Instance’s Public IP Address**:
   - Go to the **EC2 Dashboard** and select the instance.
   - Copy the **IPv4 Public IP** of the instance. This is the IP address you will use to route traffic.

2. **Create an A Record in Route 53** to Route Traffic:
   - Go to the **Route 53 Console** and select **Hosted zones**.
   - Click on the hosted zone you created earlier (e.g., `xyzcorporation.com`).
   - Click **Create Record Set**.
   
3. **Configure the DNS Record**:
   - **Name**: Enter the subdomain name you want to route traffic to (e.g., `www` for `www.xyzcorporation.com` or `app` for `app.xyzcorporation.com`).
   - **Type**: Choose **A - IPv4 address**.
   - **Value**: Paste the **public IP address** of the EC2 instance (e.g., `12.34.56.78`).
   - **TTL (Time to Live)**: Set it to the default value (e.g., 300 seconds).
   - Click **Create**.

---

### **Step 4: Test the DNS Setup**

1. Open your web browser and type the **domain name** (e.g., `http://www.xyzcorporation.com`).
   - If you configured the DNS properly, it should route traffic to the Apache web server running on your EC2 instance.
   - You should see the Apache test page or your custom web page if you modified the `index.html` file in the Apache server.

---

### **Summary of Tasks Completed:**

| Task                                                           | Status       |
|---------------------------------------------------------------|--------------|
| Used the **Route 53 hosted zone** created earlier            | ✅ Completed |
| Routed traffic to an **EC2 instance** with Apache web server using its public IP address | ✅ Completed |

---

By following these steps, XYZ Corporation will be able to route web traffic to the EC2 instance hosting their Apache web server using AWS Route 53. 
