 Problem Statement:
 You work for XYZ Corporation. To maintain the security of the AWS account and the resources you have been asked to implement a solution that can help easily recognize and monitor the different users.
 
 Tasks To Be Performed:
 1. Create 4 IAM users named “Dev1”, “Dev2”, “Test1”, and “Test2”.
 2. Create 2 groups named “Dev Team” and “Ops Team”.
 3. Add Dev1 and Dev2 to the Dev Team.
 4. Add Dev1, Test1 and Test2 to the Ops Team.

---

## **Task 1: Create 4 IAM users named “Dev1”, “Dev2”, “Test1”, and “Test2”.**

### Steps:

1. **Log in to the AWS Management Console**.
2. Go to **IAM** (Search for IAM in the Services search bar).
3. In the **IAM dashboard**, click on **Users** on the left sidebar.
4. Click on the **Add user** button.
5. For each user:
   - **User name**: Enter the username as `Dev1`, `Dev2`, `Test1`, and `Test2`.
   - **Access type**: Choose **Programmatic access** if you need API access (optional for each user based on your requirement).
   - **Console access**: If you want the users to have access to the AWS Management Console, check this box and set a password.
6. Click **Next: Permissions**.
7. You can skip attaching permissions for now and proceed to the **Next** button to review and create users.
8. Click **Create users**.
9. Save the **Access key ID** and **Secret access key** for each user if you selected programmatic access.

---

## **Task 2: Create 2 Groups named “Dev Team” and “Ops Team”.**

### Steps:

1. In the **IAM dashboard**, click on **Groups** in the left sidebar.
2. Click **Create New Group**.
3. **Group name**: Enter `Dev Team` and click **Create Group**.
4. Repeat the process for the second group:
   - **Group name**: Enter `Ops Team` and click **Create Group**.

---

## **Task 3: Add Dev1 and Dev2 to the Dev Team.**

### Steps:

1. In the **IAM dashboard**, click on **Users**.
2. Select `Dev1` from the user list.
3. Under the **Groups** tab, click **Add user to groups**.
4. Select the **Dev Team** and click **Add to Groups**.
5. Repeat the same steps for `Dev2` to add them to the **Dev Team**.

---

## **Task 4: Add Dev1, Test1, and Test2 to the Ops Team.**

### Steps:

1. In the **IAM dashboard**, click on **Users**.
2. Select `Dev1` from the user list.
3. Under the **Groups** tab, click **Add user to groups**.
4. Select the **Ops Team** and click **Add to Groups**.
5. Repeat the same steps for `Test1` and `Test2` to add them to the **Ops Team**.

---

### **Verification Steps:**

- **Check Users in Groups**: You can go to **IAM > Groups**, click on each group (Dev Team, Ops Team), and verify the list of users added to the respective groups.
  
- **Check Permissions**: By default, newly created users won't have permissions. You can assign appropriate permissions based on the roles for **Dev Team** and **Ops Team** using IAM policies.

---

## **Summary of Tasks Completed:**

| Task                                                       | Status       |
|------------------------------------------------------------|--------------|
| Created 4 IAM users: “Dev1”, “Dev2”, “Test1”, and “Test2”  | ✅ Completed |
| Created 2 IAM groups: “Dev Team” and “Ops Team”            | ✅ Completed |
| Added Dev1 and Dev2 to the Dev Team                        | ✅ Completed |
| Added Dev1, Test1, and Test2 to the Ops Team               | ✅ Completed |

---
