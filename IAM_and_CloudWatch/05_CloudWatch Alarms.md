 Problem Statement:
 You work for XYZ Corporation. To maintain the security of the AWS account and the resources you have been asked to implement a solution that can help easily recognize and monitor the different users. Also, you will be monitoring the machines created by these users for any errors or misconfigurations.
 
 Tasks To Be Performed:
 1. Create a CloudWatch billing alarm which goes off when the estimated charges go above $500.
 2. Create a CloudWatch alarm which goes off to an Alarm state when the CPU utilization of an EC2 instance goes above 65%. Also add an SNS topic so that it notifies the person when the threshold is crossed.

---
To implement the solution for **XYZ Corporation** that involves creating CloudWatch billing alarms and EC2 CPU utilization alarms with notifications, follow these steps.

---

### **Task 1: Create a CloudWatch Billing Alarm (for Charges Above $500)**

1. **Log in to the AWS Management Console**.
2. Open **CloudWatch** from the AWS Services menu or search bar.
3. On the left-hand side, click **Alarms**.
4. Click the **Create Alarm** button.
   
#### **Step 1: Set Up Billing Metric**
1. In the **Create Alarm** wizard, under the **Select metric** page, click on **Browse**.
2. Click on **Billing** > **Total Estimated Charge**.
3. You'll see various billing metrics like **EstimatedCharges** for the **USD** currency.
4. Select the **EstimatedCharges** metric and click **select metric**.

#### **Step 2: Set the Threshold for $500**
1. Set the **threshold** to $500 in the **Define the alarm** section.
   - Choose **Static threshold**.
   - Set the threshold type to **Greater/Equal**.
   - Enter `500` for the value.
   
#### **Step 3: Configure the Alarm Actions**
1. In the **Actions** section, choose **Send a notification** if the alarm is triggered.
2. You can either create a new SNS topic to receive notifications or select an existing one if you already have it set up.
   - If you don't have an SNS topic, click **Create new SNS topic**, name the topic (e.g., `Billing-Alert-Topic`), and provide your email to receive the notification.
   
3. **Review** the alarm settings and click **Create alarm**.

Once this alarm is created, CloudWatch will monitor your AWS billing, and the alarm will trigger when the estimated charges exceed $500.

---

### **Task 2: Create a CloudWatch Alarm for EC2 CPU Utilization Above 65% with SNS Notification**

#### **Step 1: Create the Alarm for EC2 CPU Utilization**

1. **Open CloudWatch** again.
2. In the left sidebar, click on **Alarms** and then click **Create Alarm**.
3. Click **Select metric** and then go to **EC2** > **Per-Instance Metrics**.
4. Find and select the **CPUUtilization** metric for the EC2 instance you want to monitor (make sure the EC2 instance is running).
5. Select the **CPUUtilization** metric and click **select metric**.

#### **Step 2: Define the Alarm Threshold**
1. In the **Create Alarm** page, set the **threshold type** to **Greater than**.
2. Set the threshold value to **65** (this means the alarm will trigger if CPU utilization exceeds 65%).
   
   - **Period**: Set it to **1 minute** to get frequent updates (or 5 minutes if you'd like a broader interval).
   
#### **Step 3: Set Up Alarm Actions (SNS Notification)**

1. Under the **Actions** section, check **Send a notification**.
2. **Create an SNS topic** or use an existing one:
   - If creating a new SNS topic, click **Create new SNS topic**.
   - Name it something like `EC2-CPU-Alert-Topic`.
   - Provide an **email address** to receive notifications when the alarm state is triggered.
   
3. You will receive an email to confirm the subscription to the SNS topic. Make sure to **confirm the subscription** in your email inbox.

#### **Step 4: Review and Create the Alarm**
1. Review your settings:
   - Threshold = **CPU Utilization > 65%**.
   - Period = **1 minute** (or your preferred time).
   - SNS Topic for notifications.
2. Click **Create alarm**.

This alarm will now notify the specified email address whenever the CPU utilization of the selected EC2 instance exceeds 65%.

---

### **Summary of Tasks Completed:**

| Task                                                          | Status       |
|---------------------------------------------------------------|--------------|
| Created **CloudWatch Billing Alarm** for charges above $500    | ✅ Completed |
| Created **CloudWatch EC2 CPU Utilization Alarm** > 65% with SNS notification | ✅ Completed |

---

### **Additional Recommendations:**

- **Testing**: Make sure to test the alarms after setting them up. You can simulate the alarm by increasing the load on your EC2 instance temporarily or by checking your estimated charges in the Billing Dashboard.
  
- **SNS Subscription**: If you're using SNS for notifications, ensure that all recipients confirm their subscription via email.
