# Case Study – Multi-Tier Architecture

## Problem Statement:
You work for XYZ Corporation, which is preparing to launch a new web-based application. The development team has completed the code but it hasn't been tested yet. The team needs a web server for testing, but the system admins are unavailable.

## Tasks To Be Performed:
### 1. **Web Tier**:
   - Launch an instance in a **public subnet**.
   - This instance should allow **HTTP** and **SSH** from the internet.

### 2. **Application Tier**:
   - Launch an instance in a **private subnet** of the web tier.
   - This instance should allow only **SSH** access from the **public subnet** of Web Tier-3.

### 3. **DB Tier**:
   - Launch an **RDS MySQL instance** in a **private subnet**.
   - The RDS instance should allow connections on **port 3306** only from the private subnet of the **Application Tier-4**.

---


### 4. **Route 53 Configuration**:
   - Set up a **Route 53 hosted zone** and direct traffic to the **EC2 instance**.

## Additional Requirements:
### 1. **Testing Automation for the Development Team**:
   - Provide a solution that allows the **development team** to test their code without needing system admins.
   - The solution should allow the development team to focus on testing rather than provisioning, configuring, and updating the resources needed for testing.

### 2. **RDS Instance Retention**:
   - Ensure that when the development team deletes the stack, the **RDS DB instances** are **not deleted**.
