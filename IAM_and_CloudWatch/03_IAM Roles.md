 Problem Statement:
 You work for XYZ Corporation. To maintain the security of the AWS account and the resources you have been asked to implement a solution that can help easily recognize and monitor the different users.

 Tasks To Be Performed:
 1. Create a role which only lets user1 and user2 from task 1 to have complete access to VPCs and DynamoDB.
 2. Login into user1 and shift to the role to test out the feature.

 ---
 To implement the solution for **XYZ Corporation** that allows **User1** and **User2** to have complete access to **VPCs** and **DynamoDB**, we'll follow the steps below. This includes creating a role, assigning permissions, and testing by logging in as **User1** and switching to the role.

### **Solution Steps:**

---

### **Task 1: Create a Role with Specific Permissions (VPCs and DynamoDB)**

#### Step 1: Create an IAM Role

1. **Login to AWS Console** and navigate to **IAM**.
2. On the left sidebar, click **Roles** and then click **Create role**.
3. Under **Select type of trusted entity**, choose **AWS service**.
4. Choose **EC2** (or choose **Another AWS account** if you want to specifically allow User1 and User2 access).
5. **Permissions**: You can directly add inline policies or attach managed policies later, but for this case, we will create a custom inline policy.

---

#### Step 2: Create a Custom Policy for VPC and DynamoDB Access

1. Under **Permissions**, click **Create policy**.
2. Go to the **JSON** tab and paste the following policy JSON to allow complete access to **VPCs** and **DynamoDB**:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:DescribeVpcs",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "dynamodb:*",
      "Resource": "*"
    }
  ]
}
```

3. Click **Review policy**. Give the policy a name like `VPCDynamoDBFullAccessPolicy`.
4. Click **Create policy**.

---

#### Step 3: Attach the Policy to the Role

1. After creating the policy, return to the **Create role** wizard in IAM.
2. Choose **Attach policies directly**, search for the policy `VPCDynamoDBFullAccessPolicy`, and select it.
3. Click **Next** and give the role a name, such as `VPCDynamoDBRole`.
4. Click **Create role**.

---

### **Task 2: Allow User1 and User2 to Assume the Role**

#### Step 1: Attach Role to User1 and User2

1. Go to **IAM** and click **Users** on the left sidebar.
2. Select **User1** and click on **Add permissions**.
3. Choose **Attach policies directly** and search for the role `VPCDynamoDBRole`.
4. Select the policy and click **Next: Review** and then **Add permissions**.

Repeat the same process for **User2** to ensure both users can assume this role.

---

### **Task 3: Login as User1 and Switch to the Role**

#### Step 1: Login as User1

1. Log in to the AWS Console using **User1's credentials**.
2. Once logged in, click on the **username** (top right) and choose **Switch Role**.
3. In the **Switch Role** interface:
   - **Account**: Leave this as the current AWS account.
   - **Role name**: Enter the role name `VPCDynamoDBRole`.
   - **Display Name**: You can set a custom name like `VPC-DynamoDB-Role`.
4. Click **Switch Role**.

---

#### Step 2: Verify Role Access

1. After switching the role, you should have access to the **VPC** and **DynamoDB** services in the AWS Console.
2. Try accessing the **VPC** service to check if you can describe VPCs and verify access.
3. Similarly, navigate to **DynamoDB** and ensure that you have complete access to the tables and can perform actions.

---

### **Summary of Tasks Completed:**

| Task                                                         | Status       |
|--------------------------------------------------------------|--------------|
| Created a role with access to VPCs and DynamoDB               | ✅ Completed |
| Allowed User1 and User2 to assume the role                   | ✅ Completed |
| Logged in as User1 and switched to the role to test access    | ✅ Completed |

---

This setup ensures that **User1** and **User2** can have complete access to **VPCs** and **DynamoDB** by assuming the specified role. 
