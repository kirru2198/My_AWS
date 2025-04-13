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
