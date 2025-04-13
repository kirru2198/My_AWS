 Problem Statement:
 You work for XYZ Corporation that uses on premise solutions and a limited number of systems. With the increase in requests in their application, the load also increases. So, to handle the load the corporation has to buy more systems almost on a regular basis. Realizing the need to cut down the expenses on systems, they decided to move their infrastructure to AWS.
 
 Tasks To Be Performed:
 1.
 - a. Create a user account that can login to the console
 - b. Create a group and make sure that the group can only launch and stop EC2 instances using that previously created account
 2. 
 - a. Provide permission to let the user of a previously created account to create VPCs, Subnets, NACL and security groups
 - b. Further add the permission so that the user can create RDS instance
 - c. Explore security options to protect the AWS resources and secure the permissions provided to the group

---

To implement the solution for **XYZ Corporation**, which is moving its infrastructure to **AWS**, we need to set up user accounts, permissions, and security best practices as outlined in the tasks. Here’s how we can break down the solution:

---

### **Task 1: Create a User Account and a Group with EC2 Permissions**

#### **1a. Create a User Account for AWS Console Login**

1. **Login to AWS Console** as an administrator.
2. Navigate to **IAM** (Identity and Access Management).
3. In the left sidebar, click on **Users** and then click **Add user**.
4. **Set the user details**:
   - **User name**: Choose a meaningful name for the user (e.g., `user-ec2`).
   - **Access type**: Choose **AWS Management Console access**.
   - **Console password**: Set a password or opt to auto-generate one (you can email it to the user later).

5. Click **Next: Permissions** to proceed.

---

#### **1b. Create a Group with EC2 Launch and Stop Permissions**

1. **Create a new group** for the user by selecting **Create group**.
2. In the **Create Group** window, name the group (e.g., `EC2-Management-Group`).
3. **Attach permissions**:
   - Select **Attach policies directly**.
   - Search for and select the following **managed policy**:
     - **`AmazonEC2FullAccess`** (This will allow launching and stopping EC2 instances).
     - Alternatively, you can create a custom policy with only the required permissions to launch and stop EC2 instances.

Here is an example of a **custom policy** that allows users to launch and stop EC2 instances:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:DescribeInstances",
                "ec2:DescribeInstanceStatus"
            ],
            "Resource": "*"
        }
    ]
}
```

4. **Review** the group and click **Create group**.

5. **Add user to the group**:
   - In the **Add user to group** section, select the group `EC2-Management-Group` and proceed to the next step.
6. **Review and create the user**.
7. The user will now be able to log into the console and only have permissions to launch and stop EC2 instances.

---

### **Task 2: Grant Permissions for VPCs, Subnets, NACLs, Security Groups, and RDS**

#### **2a. Provide Permissions to Create VPCs, Subnets, NACLs, and Security Groups**

1. **Go to IAM**, and then navigate to **Policies**.
2. **Create a custom policy** that allows the creation of VPCs, subnets, NACLs, and security groups. Below is an example of a policy that grants these permissions:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:CreateVpc",
                "ec2:DescribeVpcs",
                "ec2:CreateSubnet",
                "ec2:DescribeSubnets",
                "ec2:CreateNetworkAcl",
                "ec2:DescribeNetworkAcls",
                "ec2:CreateSecurityGroup",
                "ec2:DescribeSecurityGroups"
            ],
            "Resource": "*"
        }
    ]
}
```

3. **Attach the custom policy** to the previously created **group** (`EC2-Management-Group`).

---

#### **2b. Provide Permissions to Create RDS Instances**

1. **Create a custom policy** to allow the user to create **RDS instances**. Here's an example policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "rds:CreateDBInstance",
                "rds:DescribeDBInstances",
                "rds:ModifyDBInstance",
                "rds:DeleteDBInstance"
            ],
            "Resource": "*"
        }
    ]
}
```

2. **Attach this policy** to the same group (`EC2-Management-Group`), so the user can also create and manage RDS instances.

---

### **Task 3: Explore Security Options to Protect AWS Resources**

#### **3a. Use IAM Roles and Policies for Fine-Grained Permissions**

1. **Use least privilege access**: Make sure each user or group only has the necessary permissions for the tasks they need to perform. For example:
   - **Read-only access** for monitoring tasks.
   - **Limited write permissions** for resource creation (e.g., EC2 or RDS).

2. **Grant permissions only to necessary resources**:
   - In the IAM policies above, we used `"Resource": "*"` for simplicity. In practice, it's better to specify more granular resources (e.g., limiting permissions to specific VPCs, EC2 instances, or RDS instances).

#### **3b. Enable MFA (Multi-Factor Authentication)**

1. **Enable MFA** for the newly created user to enhance account security.
   - In the **IAM Console**, go to **Users**, select the user you created, and click on the **Security credentials** tab.
   - Under **Multi-factor authentication (MFA)**, click **Activate MFA** and follow the instructions to enable it using a virtual MFA device (e.g., Google Authenticator or Authy).

#### **3c. Set Up CloudTrail for Auditing**

1. **Enable AWS CloudTrail** to log all activities related to EC2, RDS, and other resources.
2. Go to **CloudTrail** in the AWS Console, and click **Create trail**.
3. Ensure that **all regions** are selected and log data is stored in an S3 bucket for auditing.

#### **3d. Use IAM Access Analyzer**

1. Use **IAM Access Analyzer** to ensure that resources are not publicly accessible and that no unintended access is granted.
2. Navigate to **IAM > Access Analyzer** in the console and create an analyzer to check for any open access.

#### **3e. Limit Access to Specific IPs or Networks**

1. For highly sensitive operations (like creating or managing RDS instances), **restrict access to specific IPs** or **VPCs** by using **IAM policy conditions**.
2. Example to limit access to a specific VPC:
```json
"Condition": {
    "StringEquals": {
        "aws:SourceVpc": "vpc-xxxxxxxx"
    }
}
```

---

### **Summary of Tasks Completed:**

| Task                                                      | Status       |
|-----------------------------------------------------------|--------------|
| Created a user with EC2 permissions (launch/stop instances) | ✅ Completed |
| Created a group with the above permissions                 | ✅ Completed |
| Provided permission to create VPCs, subnets, NACLs, and security groups | ✅ Completed |
| Allowed user to create RDS instances                      | ✅ Completed |
| Implemented security measures like MFA, IAM Access Analyzer, and CloudTrail | ✅ Completed |

---

By following these steps, you've successfully set up the required user accounts, permissions, and security options for **XYZ Corporation** in AWS.
