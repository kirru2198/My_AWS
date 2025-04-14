 Problem Statement:
 You work for XYZ Corporation. To maintain the security of the AWS account and the resources you have been asked to implement a solution that can help easily recognize and monitor the different users.

 Tasks To Be Performed:
 1. Create a role which only lets user1 and user2 from task 1 to have complete access to VPCs and DynamoDB.
 2. Login into user1 and shift to the role to test out the feature.

---

## ✅ **Objective Recap:**
- ✅ Create a role that:
  - Can **only** be assumed by `user1` and `user2`
  - Grants **full access** to **VPC** and **DynamoDB**
- ✅ Test by logging in as `user1` and switching to the new role

---

## 👣 Step-by-Step Guide (Visual Mode)

### 🔷 Step 1: Create the Role

1. Go to the **IAM Console**
2. In the left menu, click **Roles > Create role**
3. **Trusted Entity Type**: Select **AWS Account**
4. Choose **This account**
5. Scroll down and **check the box**: "Require external ID" → Leave it **unchecked**
6. Click **Next**

---

### 🔷 Step 2: Attach Permissions

1. In the **Add permissions** screen:
2. Click **Create policy** (this opens in a new tab)

#### ✅ In the new tab, do the following:
1. Select **Visual editor**
2. **Service**: Add **EC2** and **DynamoDB**
3. Select **All EC2 actions** (for VPC control, EC2 covers VPC APIs like `CreateVpc`, `DescribeVpcs`, etc.)
4. Select **All DynamoDB actions**
5. **Resources**: Select **All resources**
6. Click **Next**
7. Name the policy: `FullAccess-VPC-DynamoDB`
8. Click **Create policy**

⬅️ Go back to the role creation tab.

9. Click **Refresh**, search for and attach `FullAccess-VPC-DynamoDB`
10. Click **Next**

---

### 🔷 Step 3: Name and Tag Role

- **Role Name**: `SwitchRole-VPC-DynamoDB`
- (Optional) Add a tag: `Project: SecurityAudit`
- Click **Create role**

---

### 🔷 Step 4: Trust Relationship – Limit to `user1` and `user2`

Now we’ll edit the **trust policy** to allow **only `user1` and `user2` to assume this role**.

1. Go to **IAM > Roles > SwitchRole-VPC-DynamoDB**
2. Select the **Trust relationships** tab
3. Click **Edit trust policy**

Replace the default trust policy with this:

```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Principal": {
				"AWS": [
					"arn:aws:iam::445567101219:user/Dev1",
					"arn:aws:iam::445567101219:user/Dev2"
				]
			},
			"Action": "sts:AssumeRole"
		}
	]
}
```

> 🧠 Replace `<account-id>` with your **12-digit AWS Account ID**

Click **Update policy**.

---

## 🧪 Step 5: Test by Switching Roles as `user1`

### Login as `user1`:

1. Login to AWS Console as `user1`
2. In the top-right corner, click your name → **Switch Role**
3. Fill in the details:
   - **Account ID**: `<your account id>`
   - **Role name**: `SwitchRole-VPC-DynamoDB`
   - (Optional) Display name / color
4. Click **Switch Role**

You are now acting as `user1` assuming the role!

### ✅ Test It Out
Try to:
- Navigate to **VPC dashboard** → Create a VPC
- Navigate to **DynamoDB** → Create a table

You **should have full access** to both services.

---

## ✅ Summary

| Task                         | Status     |
|------------------------------|------------|
| Created IAM Role             | ✅ Done    |
| Restricted trust to 2 users  | ✅ Done    |
| Attached correct permissions | ✅ Done    |
| Tested with user1            | ✅ Done    |
