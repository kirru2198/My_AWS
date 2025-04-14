Working for an organization, you are required to provide them a safe and secure environment for the deployment of their resources. They might require different types of connectivity. Implement the following to fulfill the requirements of the company.
 
Tasks To Be Performed:
1. Create 2 EC2 instances in any public subnet of any VPC and name them Master and Client.
2. Using security groups, make sure that the Client instance can only be accessed (SSH) through the Master instance.

---
To implement a **secure environment** where:

- The **Client EC2 instance** can **only be SSH'd via the Master EC2 instance**,  
- And **not directly from the internet**,

You’ll need to use **security groups** and **bastion host architecture**. Here's how to set it up in a secure and structured way.

---

## ✅ **Architecture Overview**

- **Master EC2** (acts as a **bastion host**) → Accessible via SSH from the internet
- **Client EC2** → **NOT publicly accessible**, only accessible from the **Master EC2's private IP**

---

## 🧱 **Step-by-Step Implementation (AWS Console)**

### **1. Prerequisites**
- You have at least one VPC with a **public subnet**
- A **key pair** for SSH access

---

### **2. Create EC2 Instances**

#### a. **Master EC2 (Bastion Host)**
- Launch an EC2 instance in a **public subnet**
- Amazon Linux 2 or Ubuntu
- Attach:
  - **Security group: `SG-Master`**
  - Key pair: Your SSH key
- Allow:
  - **Inbound SSH (port 22)** from your **public IP**

#### b. **Client EC2**
- Launch another EC2 instance in the **same VPC**, any subnet (can be private or public)
- Attach:
  - **Security group: `SG-Client`**
- Do **NOT** enable public IP
- Allow:
  - **Inbound SSH (port 22)** only from the **private IP of the Master EC2**

---

### **3. Security Group Configuration**

#### ✅ SG-Master
| Type  | Protocol | Port | Source        |
|-------|----------|------|---------------|
| SSH   | TCP      | 22   | Your IP (e.g., `203.0.113.0/32`) |

#### ✅ SG-Client
| Type  | Protocol | Port | Source        |
|-------|----------|------|---------------|
| SSH   | TCP      | 22   | SG-Master (Security group of Master EC2) |

> To allow SG-based access:
- In **Source**, type `sg-xxxxxxx` (the **security group ID** of Master)
- This allows EC2 with `SG-Master` to SSH into `SG-Client`

---

### **4. Connect to the Client Instance**

**Steps:**

1. SSH into the **Master EC2**:
   ```bash
   ssh -i mykey.pem ec2-user@<Master_Public_IP>
   ```

2. From inside the Master EC2, SSH to the **Client EC2** using **its private IP**:
   ```bash
   ssh -i mykey.pem ec2-user@<Client_Private_IP>
   ```

> Ensure you copy your SSH key to the Master instance (or use `scp`) if needed.

---

## 🔒 Result

- **Master EC2** is exposed to the internet (**only for SSH**)
- **Client EC2** is **not exposed to the public**
- Access to Client is only via **Master** (bastion host)
- You're following the principle of **least privilege**

---
