 Problem Statement:
 Working for an organization, you are required to provide them a safe and secure environment for thedeployment of their resources. They might require different types of connectivity. Implement the following to fulfill the requirements of the company.
 Tasks To Be Performed:
 1. Create 2 VPCs in the North Virginia region named MYVPC1 and MYVPC2
 2. Create one VPC in the Oregon region named VPCOregon1
 3. Create a peering connection between MYVPC1 and MYVPC2
 4. Create a peering connection between MYVPC2 and VPCOregob1
---
To fulfill the given requirements, you'll need to use **Amazon VPC** services on **AWS**, specifically setting up VPCs, subnets, routing, and VPC peering. Here's a step-by-step guide to implement the tasks using the AWS Management Console, AWS CLI, or Infrastructure as Code (e.g., Terraform or CloudFormation). I’ll first provide a solution using the **AWS Management Console**, then the **AWS CLI** method.

---

## ✅ **AWS Management Console Steps**

### **1. Create Two VPCs in North Virginia (us-east-1)**

- **Go to VPC Dashboard** > **Your VPCs** > **Create VPC**
- Create:
  - **MYVPC1**
    - IPv4 CIDR block: `10.0.0.0/16`
  - **MYVPC2**
    - IPv4 CIDR block: `10.1.0.0/16`

> Make sure these CIDR blocks **do not overlap** to allow successful peering.

### **2. Create One VPC in Oregon (us-west-2)**

- Region: Switch to **Oregon (us-west-2)**
- Create:
  - **VPCOregon1**
    - IPv4 CIDR block: `10.2.0.0/16`

---

## 🔗 **3. Create Peering Connection Between MYVPC1 and MYVPC2**

- Go to **VPC Dashboard** in **us-east-1** → **Peering Connections** → **Create Peering Connection**
  - Name tag: `Peer-MYVPC1-MYVPC2`
  - Requester: **MYVPC1**
  - Accepter: **MYVPC2 (same account/region)**

- After creation, go to **Peering Connections** → **Select the new one** → Click **Actions > Accept request**

- **Update route tables** in both MYVPC1 and MYVPC2 to allow traffic:
  - MYVPC1’s route table:
    - Destination: `10.1.0.0/16`, Target: peering connection
  - MYVPC2’s route table:
    - Destination: `10.0.0.0/16`, Target: peering connection

---

## 🌐 **4. Create Peering Connection Between MYVPC2 and VPCOregon1**

> This is an **inter-region VPC peering**, supported by AWS.

- Go to **VPC Dashboard** in **us-east-1**
- Create peering connection:
  - Name tag: `Peer-MYVPC2-VPCOregon1`
  - Requester: **MYVPC2**
  - Accepter:
    - Region: **us-west-2**
    - VPC: **VPCOregon1**

- After creating it, **go to Oregon region**, **VPC dashboard**, **accept the peering request**

- **Update route tables**:
  - MYVPC2’s route table (us-east-1):
    - Destination: `10.2.0.0/16`, Target: peering connection
  - VPCOregon1’s route table (us-west-2):
    - Destination: `10.1.0.0/16`, Target: peering connection

---

## ✅ **Result**

- You now have:
  - MYVPC1 ↔ MYVPC2 (in us-east-1)
  - MYVPC2 ↔ VPCOregon1 (us-east-1 ↔ us-west-2)

---

## 🧠 Optional: AWS CLI Script (Bonus)

If you prefer using AWS CLI for automation, let me know — I can provide a script to do all of the above using CLI commands or Terraform.
