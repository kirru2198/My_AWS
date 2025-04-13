 Problem Statement:
 Working for an organization, you are required to provide them a safe and secure environment for the deployment of their resources. They might require different types of connectivity. Implement the following to fulfill the requirements of the company.
 
 Tasks To Be Performed:
 1. Create a VPC with 120.0.0.0/16 CIDR block.
 2. Create 1 public subnet 2 private subnets and make sure you connect a NAT gateway for internet connectivity to a private subnet.

---
Here's how you can **solve the problem** of securely deploying cloud infrastructure for the organization using **Amazon VPC**, **Subnets**, **NAT Gateway**, and proper routing:

---

## ✅ Problem Statement Solution

### You need to:
- Create a **VPC** with a `120.0.0.0/16` CIDR block
- Add:
  - **1 Public Subnet**
  - **2 Private Subnets**
- Set up a **NAT Gateway** so private subnets can access the internet securely

---

## 🛠️ Step-by-Step AWS Implementation

---

### 🔹 Step 1: Create a VPC

1. Go to **VPC Console** → **Your VPCs** → **Create VPC**
2. Enter:
   - **Name tag**: `MyCustomVPC`
   - **IPv4 CIDR block**: `120.0.0.0/16`
   - Tenancy: default
3. Click **Create VPC**

---

### 🔹 Step 2: Create Subnets

#### ➤ Public Subnet

1. Go to **Subnets** → **Create Subnet**
2. Choose your **VPC: MyCustomVPC**
3. Add:
   - **Subnet Name**: `PublicSubnet`
   - **Availability Zone**: e.g., `us-east-1a`
   - **IPv4 CIDR block**: `120.0.1.0/24`
4. Click **Create**

#### ➤ Private Subnet 1

1. Name: `PrivateSubnet1`
2. AZ: e.g., `us-east-1b`
3. CIDR: `120.0.2.0/24`

#### ➤ Private Subnet 2

1. Name: `PrivateSubnet2`
2. AZ: e.g., `us-east-1c`
3. CIDR: `120.0.3.0/24`

---

### 🔹 Step 3: Create an Internet Gateway (IGW) for Public Subnet

1. Go to **Internet Gateways** → **Create Internet Gateway**
   - Name: `MyIGW`
2. Click **Create**, then select and **Attach** to your `MyCustomVPC`

---

### 🔹 Step 4: Modify the Route Table for Public Subnet

1. Go to **Route Tables**
2. Identify the route table attached to your VPC (or create a new one for public use)
3. Name it: `PublicRouteTable`
4. Edit Routes → Add route:
   - **Destination**: `0.0.0.0/0`
   - **Target**: `Internet Gateway → MyIGW`
5. **Associate** this route table with the `PublicSubnet`

---

### 🔹 Step 5: Allocate an Elastic IP (EIP) for NAT Gateway

1. Go to **Elastic IPs** → **Allocate Elastic IP**
2. Click **Allocate**
3. Note the EIP — you’ll use it when creating the NAT Gateway

---

### 🔹 Step 6: Create NAT Gateway in the Public Subnet

1. Go to **NAT Gateways** → **Create NAT Gateway**
2. Name: `MyNATGateway`
3. Subnet: `PublicSubnet`
4. Elastic IP: select the one you just allocated
5. Click **Create NAT Gateway**

---

### 🔹 Step 7: Create & Associate Route Table for Private Subnets

1. Go to **Route Tables** → **Create Route Table**
   - Name: `PrivateRouteTable`
   - VPC: `MyCustomVPC`
2. Edit Routes → Add:
   - **Destination**: `0.0.0.0/0`
   - **Target**: `NAT Gateway → MyNATGateway`
3. **Associate** this route table with both:
   - `PrivateSubnet1`
   - `PrivateSubnet2`

---

### ✅ Final Architecture Summary:

| Resource            | Details              |
|---------------------|----------------------|
| **VPC**             | `120.0.0.0/16`       |
| **Public Subnet**   | `120.0.1.0/24`       |
| **Private Subnet 1**| `120.0.2.0/24`       |
| **Private Subnet 2**| `120.0.3.0/24`       |
| **Internet Gateway**| Attached to VPC      |
| **NAT Gateway**     | In Public Subnet, routes for private subnets |
| **Public Route Table** | Routes to Internet Gateway |
| **Private Route Table** | Routes to NAT Gateway |

---

### 🧪 Testing

- Launch a test **EC2 instance** in each subnet.
- In **public subnet**, verify internet access directly.
- In **private subnet**, verify internet access only through the **NAT Gateway** (e.g., `yum update` or `ping google.com`).
- Ensure **no direct public IP** is given to private instances for better security.

---
