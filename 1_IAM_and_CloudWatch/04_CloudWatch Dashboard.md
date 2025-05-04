 Problem Statement:
 You work for XYZ Corporation. To maintain the security of the AWS account and the resources you have been asked to implement a solution that can help easily recognize and monitor the different users. Also, you will be monitoring the machines created by these users for any errors or misconfigurations.
 
 Tasks To Be Performed:
 1. Create a dashboard which lets you check the CPU utilization and networking for a particular EC2 instance.

 ---
To implement a solution that allows you to monitor the **CPU utilization** and **networking** of a particular EC2 instance, you will need to create a **CloudWatch Dashboard** in AWS. This dashboard will allow you to view real-time metrics of your EC2 instances and easily monitor their performance.

### **Steps to Create a CloudWatch Dashboard for Monitoring EC2 Metrics**

---

### **Task 1: Set Up CloudWatch Monitoring for EC2 Instance**

Before you create the CloudWatch dashboard, you need to ensure that your EC2 instance is sending the necessary metrics (CPU utilization, networking, etc.) to **Amazon CloudWatch**.

By default, **EC2 instances** have the **basic CloudWatch metrics** enabled, including **CPU utilization**, **disk I/O**, **networking**, and **status checks**. For more granular metrics like **detailed monitoring**, you need to enable **detailed monitoring** for the EC2 instance.

#### **Step 1: Enable Detailed Monitoring (Optional)**

1. Go to the **EC2 Dashboard** in the AWS Management Console.
2. In the left sidebar, click **Instances** and then select the EC2 instance you want to monitor.
3. In the **Monitoring** tab, click **Enable detailed monitoring** (if not already enabled). This will send metrics at 1-minute intervals instead of the default 5-minute interval.

---

### **Task 2: Create CloudWatch Dashboard**

1. **Log in to AWS Console** and open **CloudWatch** from the search bar or **Services** menu.

2. In the **CloudWatch Dashboard**:
   - On the left-hand side, click **Dashboards**.
   - Click the **Create dashboard** button.

3. **Name the Dashboard**: Choose a name for your dashboard, such as `EC2-Monitoring-Dashboard`.

4. **Add a widget**:
   - After naming the dashboard, click **Add widget**.
   - Choose **Line** or **Stacked area** for the widget type (you can adjust this later based on your preference).
   - Select **Metrics** as the source for the widget.

5. **Select EC2 Metrics**:
   - In the **Browse** section, select **EC2**.
   - Select **Per-Instance Metrics** to view metrics for specific EC2 instances.

6. **Select the EC2 instance you want to monitor**:
   - You will see a list of EC2 instances. Choose the one you want to monitor.
   - You can add multiple metrics (e.g., CPU utilization, NetworkIn, NetworkOut) in the same widget or use separate widgets for each metric.

7. **Choose Metrics**:
   - **CPUUtilization**: To monitor CPU utilization, check the box for the **CPUUtilization** metric.
   - **NetworkIn and NetworkOut**: To monitor networking, select **NetworkIn** (for incoming traffic) and **NetworkOut** (for outgoing traffic).

8. **Add the Metrics**:
   - Click **Create widget** to add the selected metrics to your dashboard.
   
9. **Repeat** (Optional):
   - You can add multiple widgets for different metrics or for multiple EC2 instances.
   - For example, add a widget for **Disk I/O** if you want to monitor disk activity as well.

---

### **Task 3: Customize and Save the Dashboard**

1. **Resize and Arrange Widgets**:
   - You can drag and resize the widgets on the dashboard to organize them.
   - Place the CPU, Network, and other relevant metrics in the arrangement that best suits your needs.

2. **Save the Dashboard**:
   - Once you're happy with the dashboard layout, click **Save changes**.

---

### **Task 4: View the Dashboard**

1. After saving, you will be able to view real-time metrics of the selected EC2 instance in your CloudWatch dashboard.
2. Monitor **CPU utilization**, **network traffic**, and other metrics from this dashboard.
3. You can also set up **alarms** on specific metrics (e.g., CPU utilization > 80%) if you'd like to receive notifications for specific thresholds.

---

### **Summary of Tasks Completed:**

| Task                                             | Status       |
|--------------------------------------------------|--------------|
| Enabled detailed monitoring on EC2 instance     | ✅ Completed |
| Created a CloudWatch Dashboard                  | ✅ Completed |
| Added CPU utilization and networking metrics    | ✅ Completed |
| Customized and saved the dashboard              | ✅ Completed |

---

### **Additional Recommendations**:

- **Setting Alarms**: You can set **CloudWatch Alarms** on metrics like CPU utilization or network traffic. This allows you to receive notifications if the metrics exceed a defined threshold (e.g., CPU > 80%).
  
- **Cost Optimization**: Keep in mind that detailed monitoring comes with an additional cost. Only enable detailed monitoring if it's necessary.

This setup allows you to easily monitor the health and performance of your EC2 instances directly from the CloudWatch dashboard. 
