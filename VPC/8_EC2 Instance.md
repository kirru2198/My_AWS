# AWS VPC and EC2 Instance Setup

## Launching EC2 Instances

### 1. Launch a Public EC2 Instance
- Go to the **EC2 Dashboard**.
- Click on **Launch Instances**.
- Choose an Amazon Machine Image (AMI) (e.g., Amazon Linux 2).
- Select an instance type (e.g., `t2.micro` for free tier).
- Create a new key pair or use an existing one.
- Under **Network settings**, select your custom VPC and the public subnet.
- Ensure **Auto-assign Public IP** is enabled.
- Create a new security group (e.g., `MySSHSecurityGroup`) allowing SSH (port 22) from anywhere (0.0.0.0/0).
- Click **Launch Instance**.

### 2. Launch a Private EC2 Instance
- Repeat the steps to launch another instance, but this time:
  - Select the private subnet.
  - Disable **Auto-assign Public IP**.
  - Use the same security group created for the public instance.

## Connecting to EC2 Instances

### 1. Connect to the Public Instance
- Use the SSH command:
  ```bash
  ssh -i /path/to/your/key.pem ec2-user@<Public-IP>
  ```

### 2. Connect to the Private Instance
- To connect to the private instance, first SSH into the public instance.
- Then use the private IP of the private instance:
  ```bash
  ssh -i /path/to/your/key.pem ec2-user@<Private-IP>
  ```

### Important Note
- The private instance does not have a public IP, so it cannot be accessed directly from the internet. You must connect through the public instance.

## Installing Software on Private Instance
- After connecting to the private instance, you may try to install software (e.g., HTTPD):
  ```bash
  sudo yum install httpd -y
  ```
- This will fail because the private instance does not have internet access.

## Enabling Internet Access for Private Instances
- To allow private instances to access the internet, you can set up a **NAT Gateway**. This will be covered in a future session.

## Security Groups
- Each VPC has a default security group, which is restrictive.
- You can create custom security groups to allow specific traffic (e.g., SSH access).

## Conclusion
Creating a VPC with public and private subnets allows for better security and management of resources. Understanding how to set up and connect to EC2 instances in different subnets is crucial for effective cloud architecture. Practice these steps multiple times to become proficient in managing AWS resources.
