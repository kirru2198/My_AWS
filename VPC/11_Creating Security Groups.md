SecurityGroups_EC2_Detailed_Guide

# Detailed Guide: Creating Security Groups and Configuring EC2 Instances

This guide explains step-by-step how to create and configure security groups and EC2 instances for SSH and web traffic, how security groups work, and the role of Elastic Network Interfaces (ENIs).

---

## 1. Understanding Security Groups

- **Security Groups**: Virtual firewalls that control inbound and outbound traffic for EC2 instances.
- **Stateful Nature**: If an inbound/outbound request is allowed, the corresponding response traffic is automatically allowed without extra rules.
- **Allow Rules Only**: Security groups only *allow* traffic explicitly defined. All other traffic is denied implicitly.

---

## 2. Creating Security Groups

### Step 1: Create SSH Security Group

- Go to the **EC2 Dashboard** > **Security Groups** > **Create security group**.
- Name it something like `my-ssh-security-group`.
- **VPC**: Select your default VPC (or the VPC where you want to launch your instance).
- **Inbound Rules**: Initially leave blank (we will add rules later).
- **Outbound Rules**: Leave blank for now (also add later if needed).
- Create the group.

### Step 2: Create Web Security Group

- Repeat the above steps and name it `my-web-security-group`.
- This group will manage HTTP and HTTPS traffic.
- Initially no inbound or outbound rules.
- Create the group.

---

## 3. Launching EC2 Instance

### Step 3: Set Up EC2

- Go to **EC2 Dashboard** > **Instances** > **Launch Instance**.
- Name the instance, e.g., `my-web-instance`.
- Choose an AMI (Amazon Linux 2 recommended).
- Choose Instance Type (e.g., t2.micro).
- In **Configure Instance**, select your default VPC.
- In **Add Storage**, keep default.
- In **Add Tags**, optional.
- In **Configure Security Group**, select **Select an existing security group**.
- Attach only `my-ssh-security-group` (for now).
- **Key Pair**: Select an existing or create a new key pair to SSH into the instance.
- **Advanced Details**: 
  Add this user data script for automatic HTTP server installation on boot:
  ```bash
  #!/bin/bash
  yum install httpd -y
  service httpd start
  echo "Security Groups Test" > /var/www/html/index.html
  ```
- Launch the instance.

---

## 4. Connecting to Your Instance (Initial Test)

### Step 4: Try to SSH into the instance

- Use the following command:
  ```bash
  ssh -i /path/to/key.pem ec2-user@<Your-Instance-Public-IP>
  ```
- You will **fail** because the security group currently has no inbound SSH rules allowing your IP.

### Step 5: Update inbound SSH rule

- Go to **Security Groups**.
- Select `my-ssh-security-group`.
- Edit **Inbound Rules**.
- Add a rule to allow:
  - Type: SSH
  - Protocol: TCP
  - Port Range: 22
  - Source: Your IP (or Anywhere (0.0.0.0/0) for testing, but for security, limit to your IP)
- Save rules.

### Step 6: Connect again via SSH

- Now try the SSH command again; this time it should succeed.

---

## 5. Installing and Accessing HTTP Server

### Step 7: Verify httpd service

- After successful SSH, check if HTTPD service is running:
  ```bash
  sudo service httpd status
  ```
- It should show inactive because automatic install might not have worked due to missing outbound rules.

### Step 8: Add outbound HTTP/HTTPS rules

- Go to **Security Groups**.
- Select `my-web-security-group`.
- Edit **Outbound Rules**.
- Add two rules to allow:
  - Type: HTTP | Protocol: TCP | Port: 80 | Destination: Anywhere (0.0.0.0/0)
  - Type: HTTPS | Protocol: TCP | Port: 443 | Destination: Anywhere (0.0.0.0/0)
- Save rules.

### Step 9: Attach web security group to instance

- Go to **EC2 Instances**.
- Select your instance.
- Actions > Security > **Manage Security Groups**.
- Attach `my-web-security-group` in addition to `my-ssh-security-group`.
- Save.

### Step 10: Install httpd manually if needed

- Connect via SSH and run:
  ```bash
  sudo yum install httpd -y
  sudo service httpd start
  echo "Security Groups Test" | sudo tee /var/www/html/index.html
  ```

---

## 6. Accessing the Web Server in Browser

### Step 11: Configure inbound HTTP rule

- Edit **Inbound Rules** of `my-web-security-group`.
- Add rule to allow:
  - Type: HTTP | Protocol: TCP | Port: 80 | Source: Anywhere (0.0.0.0/0)
- Save rules.

### Step 12: Open browser

- Enter your instance public IP in the browser:
  ```
  http://<Your-Instance-Public-IP>/
  ```
- You should see the message "Security Groups Test" or your default webpage.

---

## 7. Understanding Security Group Behavior

- **Multiple security groups** attached to an instance combine their rules.
- If any rule allows the traffic, it is granted.
- Security groups never explicitly block traffic; absence of rule means traffic is denied.
- Stateful: return traffic is automatically allowed.

---

## 8. Elastic Network Interfaces (ENIs)

- Each EC2 instance has one or more **ENIs** (Elastic Network Interfaces).
- Each ENI has its own private/public IP, MAC address, and security groups.
- Security groups are attached to ENIs, not directly to instances.
- ENIs can be detached and attached to other instances, providing flexibility for:
  - High availability
  - IP address persistence across instance switches
- You can create additional ENIs and assign them to instances for separation of network traffic.

---

## Summary

| Step                  | What to do                        | Why?                                       |
|-----------------------|---------------------------------|--------------------------------------------|
| Create SSH SG         | Allow SSH inbound for remote access | To securely SSH into your instance         |
| Create Web SG         | Allow HTTP/HTTPS outbound initially | To enable downloading packages and serve websites |
| Attach SGs            | Attach both SGs to the instance  | To allow both SSH access and web traffic   |
| Add inbound rules     | Allow inbound HTTP and SSH       | To allow incoming requests to the instance |
| Add outbound rules    | Allow outbound HTTP and HTTPS    | To allow instance to download packages     |
| Use ENIs              | Understand network interface concept | For flexibility and network management     |

---

## Additional Tips

- Always restrict source IP ranges in security group rules in production for better security.
- Use Elastic IPs to have static public IP addresses.
- For complex architectures, consider Network ACLs (firewall at subnet level) besides security groups.
- Security groups apply immediately on rule change — no need to restart instances.

---

This guide should give you a step-by-step, clear understanding of creating and managing security groups, launching EC2 instances, troubleshooting connectivity, and the role of network interfaces in AWS.
