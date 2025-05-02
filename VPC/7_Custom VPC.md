# Step-by-Step Guide to Creating a Custom VPC in AWS

## Step 1: Create a Custom VPC

- Go to the **VPC Dashboard** in the AWS Management Console.
- Click **Create VPC**.
- Choose **Create VPC only** (don’t use predefined templates since you want to create resources manually).
- Fill in the details:
  - **Name tag**: Give your VPC a name, e.g., `custom-vpc`.
  - **IPv4 CIDR block**: Enter `10.0.0.0/16`.  
    - Explanation: This CIDR block defines the private IP address range for this VPC, with about 65,536 possible IP addresses.
    - AWS requires the block size to be between `/16` (larger network) and `/28` (smallest).
  - **IPv6 CIDR block**: Select **No IPv6 CIDR block** for now (optional).
  - **Tenancy**: Choose **default**.  
    - Explanation: Default tenancy means that EC2 instances launched in this VPC share physical hardware with instances from other AWS accounts. Dedicated tenancy would reserve hardware exclusively for you, which costs more.

- Click **Create VPC**.

AWS will create your VPC and automatically associate a **default route table** and a **network ACL** for it.

---

## Step 2: Create Subnets

### Why Subnets?

Subnets partition your VPC’s IP address range into smaller segments, allowing you to separate your resources logically and control routing and access.

We'll create two subnets:

- **Public Subnet**: For resources that need internet access, like web servers.
- **Private Subnet**: For resources that should not be directly accessible from the internet, like databases.

### How to create Subnets:

- In the VPC dashboard, go to **Subnets** and click **Create subnet**.
- **Assign to your custom VPC**: Select the VPC you created (`custom-vpc`).

#### Create Public Subnet

- **Name tag**: `public-subnet`.
- **Availability Zone**: Select an AZ, e.g., `us-east-1a`.
- **IPv4 CIDR block**: Enter `10.0.0.0/24`.  
  - This subnet has 256 IP addresses (about 251 usable by AWS after reserving some IPs).
- Click **Create subnet**.

#### Create Private Subnet

- Click **Create subnet** again.
- **Name tag**: `private-subnet`.
- **Availability Zone**: Choose a different AZ, e.g., `us-east-1b` (for high availability).
- **IPv4 CIDR block**: Enter `10.0.1.0/24`.
- Click **Create subnet**.

---

## Step 3: Configure the Public Subnet to Auto-Assign Public IPs

Instances in the public subnet need public IP addresses to communicate with the internet.

- Select the `public-subnet` in the Subnets panel.
- Click the **Actions** dropdown → **Modify auto-assign IP settings**.
- Enable **Auto-assign IPv4 public IP**.
- Save changes.

Note: The private subnet will have this option disabled by default, which is correct.

---

## Step 4: Create and Attach an Internet Gateway (IGW)

An Internet Gateway allows your VPC to access the internet and enables your public subnet to route traffic outside AWS.

- Go to **Internet Gateways** → Click **Create internet gateway**.
- Name it, e.g., `custom-igw`.
- Click **Create**.
- Now select the IGW, and from the **Actions** dropdown, select **Attach to VPC**.
- Choose your custom VPC and click **Attach**.

---

## Step 5: Configure Route Tables

### Modify Route Table for Public Subnet

- Go to **Route Tables** in the VPC Dashboard.
- You will find your **main/default route table** automatically created for the VPC.
- Create a new route table for the **public subnet** to add internet access:
  - Click **Create route table**.
  - Name it `public-route-table`.
  - Select your custom VPC.
  - Click **Create**.

- Select the new `public-route-table`.
- In the **Routes** tab, click **Edit routes** → **Add route**.
- **Destination**: `0.0.0.0/0` (meaning all IPv4 addresses).
- **Target**: Select your Internet Gateway (`custom-igw`).
- Save routes.

### Associate Public Route Table with Public Subnet

- With `public-route-table` selected, go to **Subnet associations** tab.
- Click **Edit subnet associations**.
- Select your `public-subnet`.
- Save.

### Private Subnet Routing

- The private subnet will remain associated with the **main/default route table**, which only routes traffic within the VPC and does not have a route to the internet gateway. So private subnet instances cannot access the internet directly.

---

## Step 6: Verify Your Setup

- Your **Public subnet** has:
  - Auto-assign public IP enabled.
  - A route table with a route to the internet gateway (`0.0.0.0/0` → IGW).
- Your **Private subnet** has:
  - No auto-assign public IP.
  - Route table that routes only within the VPC.

---

## Additional Important Concepts

### What Makes a Subnet Public or Private?

- A subnet is **public** if its route table includes a route to an Internet Gateway.
- A subnet is **private** if its route table does NOT include a route to the Internet Gateway.

### Tenancy Options

- **Default Tenancy**: Instances share physical hardware with instances from other accounts (cost-effective).
- **Dedicated Tenancy**: Physical hardware dedicated to one customer (more expensive).

### Why Use Multiple Availability Zones?

- Deploying resources in multiple availability zones ensures high availability and fault tolerance.
- For example, place your public subnet in `us-east-1a` and private subnet in `us-east-1b`.

### Why Do CIDR Blocks for Subnets Need to NOT Overlap?

- Overlapping CIDRs cause routing conflicts.
- Subnets within a VPC must have distinct ranges within the primary VPC CIDR block.

---

## Summary of What You Have Created

| Component             | Description                                                                            |
|-----------------------|----------------------------------------------------------------------------------------|
| VPC                   | Custom VPC `10.0.0.0/16` with default tenancy                                          |
| Public Subnet         | Subnet `10.0.0.0/24` with auto-assign public IP, associated with `public-route-table`  |
| Private Subnet        | Subnet `10.0.1.0/24` without public IPs, associated with default route table          |
| Internet Gateway (IGW) | Enables internet access, attached to VPC                                               |
| Public Route Table    | Routes `0.0.0.0/0` traffic to IGW, associated with public subnet                       |
| Default Route Table   | Handles internal routing only for private subnet                                      |

---

## What’s Next?

- Launch EC2 instances inside your **public subnet** and confirm they have internet access.
- Launch EC2 instances inside your **private subnet** and verify they can communicate internally but have no direct internet access.
- Configure a NAT Gateway in a public subnet to allow instances in the private subnet to access the internet securely if needed.

---
