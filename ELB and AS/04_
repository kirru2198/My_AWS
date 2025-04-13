 Problem Statement:
 You work for XYZ Corporation that uses on premise solutions and a limited number of systems. With the increase in requests in their application, the load also increases. So, to handle the load the corporation has to buy more systems almost on a regular basis. Realizing the need to cut down the expenses on systems, they decided to move their infrastructure to AWS.
 
Tasks To Be Performed:
 1. Manage the scaling requirements of the company by:
 a. Deploying multiple compute resources on the cloud as soon as the load increases and the CPU utilization exceeds 80%
 b. Removing the resources when the CPU utilization goes under 60%
 2. Create a load balancer to distribute the load between compute resources.
 3. Route the traffic to the company’s domain.

--- 
To help **XYZ Corporation** manage increased application load while reducing infrastructure costs, here's how you can **implement dynamic scaling and routing** using **AWS Auto Scaling, Elastic Load Balancer, and Route 53**.

---

## ✅ **Tasks Breakdown & Solution**

---

### **🟩 Task 1: Manage Scaling Based on CPU Utilization**

#### **Step 1.1: Create a Launch Template or Launch Configuration**

This will define how your EC2 instances should be created.

1. **Go to EC2 Console → Launch Templates**
2. Click **Create launch template**
   - Name: `webserver-template`
   - AMI: Use an AMI with a web server (Apache/Nginx) pre-installed
   - Instance type: `t2.micro` (or as needed)
   - Key pair: Select your existing key pair
   - Security group: Allow **port 22 (SSH)** and **port 80 (HTTP)**

#### **Step 1.2: Create Auto Scaling Group**

1. **Go to EC2 → Auto Scaling Groups → Create Auto Scaling Group**
2. Name: `webserver-asg`
3. Use the launch template you just created
4. Network: Select VPC & at least 2 subnets for high availability
5. Desired Capacity: 1
6. Minimum: 1 | Maximum: 5

#### **Step 1.3: Add Scaling Policies**

Select **Target tracking** OR **Step scaling policy**:

✅ **Use Step Scaling (based on CPU):**
- Policy 1 (Scale out): Add 1 instance if average CPU > 80% for 2 consecutive periods
- Policy 2 (Scale in): Remove 1 instance if average CPU < 60% for 2 consecutive periods

> You can also monitor and adjust these via **Amazon CloudWatch Alarms**.

---

### **🟩 Task 2: Create a Load Balancer**

To distribute traffic across the Auto Scaling EC2 instances.

#### **Step 2.1: Create Application Load Balancer (ALB)**

1. Go to **EC2 → Load Balancers → Create Load Balancer**
2. Choose **Application Load Balancer**
   - Name: `webserver-alb`
   - Scheme: Internet-facing
   - Listeners: HTTP (port 80)
3. **Select VPC & Subnets** (same as Auto Scaling)
4. Configure **Security Group** (allow HTTP access)
5. Create a **Target Group** (e.g., `webserver-target-group`)
   - Target type: Instance
   - Protocol: HTTP, Port: 80
   - Health check path: `/`
6. Register targets: **Skip this** (Auto Scaling group will auto-register)
7. Create the ALB.

#### **Step 2.2: Attach Load Balancer to Auto Scaling Group**

1. Go to **Auto Scaling Group** → Edit
2. Under **Load balancing**, choose **Attach to existing load balancer**
3. Select the **Target Group** created with ALB

---

### **🟩 Task 3: Route Traffic Using Route 53**

Route traffic to your domain (e.g., `www.xyzcorporation.com`)

#### **Step 3.1: Go to Route 53 → Hosted Zones**

If you already have a domain (e.g., from GoDaddy), you must:
- Set up a **Hosted Zone** in Route 53
- Point your domain’s name servers to Route 53 (via registrar)

#### **Step 3.2: Create an A Record**

1. In the hosted zone, click **Create Record**
2. Record type: **A – IPv4 address**
3. Choose **Alias**
4. Target: Select your **Application Load Balancer (ALB)**
5. Record name: `www` or leave blank for root domain
6. Save the record

---

### ✅ Summary of What We Did:

| Task | Description | Status |
|------|-------------|--------|
| 1a   | Auto Scaling triggers when CPU > 80% | ✅ |
| 1b   | Scale in when CPU < 60% | ✅ |
| 2    | Application Load Balancer setup | ✅ |
| 3    | DNS routing to ALB using Route 53 | ✅ |

---

### 📌 Final Notes:

- Make sure your **EC2 instances are properly configured** to serve traffic (Apache running, index.html in place).
- Test scaling with CPU stress tools or manual changes in desired capacity.
- Use **CloudWatch** to monitor scaling activity and system health.

