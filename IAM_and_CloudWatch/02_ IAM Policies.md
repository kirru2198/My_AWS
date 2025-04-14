Certainly! Here's the rewritten version of your problem statement and tasks:

---

**Problem Statement:**

You work for XYZ Corporation. To maintain the security of the AWS account and the resources, you have been asked to implement a solution that can help easily recognize and monitor the different users.

**Tasks to be Performed:**

1. Create Policy Number 1 which lets the users:
   a. Access S3 completely  
   b. Only create EC2 instances  
   c. Full access to RDS  

2. Create Policy Number 2 which allows the users to:
   a. Access CloudWatch and billing completely  
   b. Can only list EC2 and S3 resources  

3. Attach Policy Number 1 to the Dev Team from Task 1.

4. Attach Policy Number 2 to the Ops Team from Task 1.

---

To address the problem and implement the required policies and permissions for the **XYZ Corporation**, we need to:

1. **Create IAM Policies** to define the permissions.
2. **Attach Policies to Groups** (Dev Team and Ops Team).

### **Solution Overview:**

---

### **Task 1: Create Policy Number 1**

**Policy Number 1** needs to provide the following permissions:
- Full access to **S3**.
- Only permission to **create EC2 instances**.
- Full access to **RDS**.

We'll use AWS **IAM policy JSON** to define these permissions.

#### **Policy Number 1 JSON:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::*"
    },
    {
      "Effect": "Allow",
      "Action": "ec2:RunInstances",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "rds:*",
      "Resource": "*"
    }
  ]
}
```

#### **Steps to Create Policy Number 1:**
1. **Go to the AWS Console** and open **IAM**.
2. In the **IAM dashboard**, click on **Policies** on the left sidebar.
3. Click **Create Policy**.
4. Choose the **JSON** tab and paste the policy JSON above.
5. Click **Review policy**, name the policy **Policy Number 1**, and add a description (e.g., "Access to S3, create EC2 instances, and full access to RDS").
6. Click **Create policy**.

---

### **Task 2: Create Policy Number 2**

**Policy Number 2** needs to provide the following permissions:
- Full access to **CloudWatch** and **Billing**.
- Only permission to **list EC2 and S3 resources** (i.e., list, describe, etc.).

#### **Policy Number 2 JSON:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:*",
        "aws-portal:ViewBilling"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:DescribeImages",
        "s3:ListBucket"
      ],
      "Resource": "*"
    }
  ]
}
```

#### **Steps to Create Policy Number 2:**
1. In the **IAM dashboard**, click on **Policies** on the left sidebar.
2. Click **Create Policy**.
3. Choose the **JSON** tab and paste the policy JSON above.
4. Click **Review policy**, name the policy **Policy Number 2**, and add a description (e.g., "Access to CloudWatch and Billing, list EC2 and S3 resources").
5. Click **Create policy**.

---

### **Task 3: Attach Policy Number 1 to the Dev Team**

1. Go to **IAM** and click on **Groups**.
2. Find the **Dev Team** group (from **Task 1**).
3. Click on the **Dev Team** group, then click **Attach policy**.
4. Search for **Policy Number 1** and select it.
5. Click **Attach policy**.

---

### **Task 4: Attach Policy Number 2 to the Ops Team**

1. Go to **IAM** and click on **Groups**.
2. Find the **Ops Team** group (from **Task 1**).
3. Click on the **Ops Team** group, then click **Attach policy**.
4. Search for **Policy Number 2** and select it.
5. Click **Attach policy**.

---

### **Summary of Tasks Completed:**

| Task                                                        | Status       |
|-------------------------------------------------------------|--------------|
| Created **Policy Number 1** (Access to S3, create EC2, RDS) | ✅ Completed |
| Created **Policy Number 2** (Access CloudWatch, Billing, list EC2/S3) | ✅ Completed |
| Attached **Policy Number 1** to **Dev Team**                | ✅ Completed |
| Attached **Policy Number 2** to **Ops Team**                | ✅ Completed |

---
To accomplish this using **AWS IAM (Identity and Access Management)** in **Visual Editor (Visual Mode)** within the AWS Console, here's a **step-by-step guide** for creating and attaching the two policies to the `Dev Team` and `Ops Team` IAM groups:

---

## ✅ **Policy Number 1 – Dev Team**

### 🎯 Permissions:
- ✅ Full access to **S3**
- ✅ Only ability to **create EC2 instances**
- ✅ Full access to **RDS**

---

### 👣 Steps to Create Policy 1:

1. Go to **IAM Console > Policies > Create policy**
2. Choose **Visual editor**
3. Click **Service**, and add the following:

#### **S3 – All Access**
- Service: **S3**
- Actions: **All S3 actions** (Select `All S3 actions`)
- Resources: **All resources**

---

#### **EC2 – Only Create Instances**
- Service: **EC2**
- Actions: Only select:
  - `RunInstances`
- Resources: **All resources**

---

#### **RDS – Full Access**
- Service: **RDS**
- Actions: **All RDS actions**
- Resources: **All resources**

---

4. Click **Next**, name the policy:
   - **Name**: `DevTeam-FullS3-RDS-CreateEC2`
   - **Description**: "Full access to S3 and RDS, create-only for EC2"
5. Click **Create policy**

---

### 📎 Attach Policy 1 to Dev Team:

1. Go to **IAM > Groups > Dev Team**
2. Choose **Permissions > Add permissions > Attach policies**
3. Search for `DevTeam-FullS3-RDS-CreateEC2`
4. Select it and click **Attach policy**

---

## ✅ **Policy Number 2 – Ops Team**

### 🎯 Permissions:
- ✅ Full access to **CloudWatch** and **Billing**
- ✅ Only **List** access for EC2 and S3

---

### 👣 Steps to Create Policy 2:

1. Go to **IAM Console > Policies > Create policy**
2. Choose **Visual editor**
3. Click **Service**, and add the following:

#### **CloudWatch – Full Access**
- Service: **CloudWatch**
- Actions: **All CloudWatch actions**
- Resources: **All resources**

---

#### **Billing – Full Access**
- Service: **Billing**
- Actions: **All Billing actions**
- Resources: **All resources**
- ✅ *Note*: To grant access to Billing, user **must have root permissions** or billing permissions must be enabled for IAM users.

---

#### **S3 – List Only**
- Service: **S3**
- Actions:
  - `ListAllMyBuckets`
  - `ListBucket`
- Resources: **All resources**

---

#### **EC2 – List Only**
- Service: **EC2**
- Actions:
  - `DescribeInstances`
  - `DescribeVolumes`
  - `DescribeImages`
  - (Optional: Add others under "Describe*")
- Resources: **All resources**

---

4. Click **Next**, name the policy:
   - **Name**: `OpsTeam-Monitoring-ListEC2S3`
   - **Description**: "Full access to monitoring/billing, list-only for EC2 & S3"
5. Click **Create policy**

---

### 📎 Attach Policy 2 to Ops Team:

1. Go to **IAM > Groups > Ops Team**
2. Choose **Permissions > Add permissions > Attach policies**
3. Search for `OpsTeam-Monitoring-ListEC2S3`
4. Select it and click **Attach policy**

---

✅ **Done!**  
You've now created and attached two custom IAM policies using the **Visual Editor**, precisely scoped for your Dev and Ops teams.
