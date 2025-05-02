# Detailed Notes on Creating Security Groups and EC2 Instance Configuration

## Introduction
- **Objective**: Demonstrate the creation of security groups and their impact on EC2 instance connectivity and software installation.
- **Current Setup**: Only one security group (Launch Wizard 1) exists. The goal is to create two new security groups.

## Creating Security Groups
### Step 1: Create SSH Security Group
- **Name**: `my-ssh-security-group`
- **Purpose**: To allow SSH traffic.
- **VPC**: Using the default VPC.
- **Rules**: Initially, no inbound or outbound rules are set.

### Step 2: Create Web Security Group
- **Name**: `my-web-security-group`
- **Purpose**: To allow HTTP and HTTPS traffic.
- **Rules**: Initially, no inbound or outbound rules are set.

### Note on Security Group Rules
- **Separation of Concerns**: It is better to separate security groups for different purposes (e.g., SSH and web traffic) for better management and reusability.

## Launching an EC2 Instance
### Step 3: Create EC2 Instance
- **Instance Name**: `my-web-instance`
- **Key Pair**: Use the key created earlier.
- **VPC**: Default VPC.
- **Security Group**: Select `my-ssh-security-group` for SSH access.
- **User  Data Script**:
  ```bash
  #!/bin/bash
  yum install httpd -y
  service httpd start
  echo "Security Groups Test" > /var/www/html/index.html
  ```

### Step 4: Launch the Instance
- **Observation**: The instance is running, but attempts to connect via SSH will fail due to the lack of inbound rules.

## Testing Connectivity
### Step 5: Attempt to Connect to EC2 Instance
- **Connection Command**: Use SSH command to connect.
- **Result**: Connection fails because the security group has no inbound rules to allow SSH traffic.

### Step 6: Modify Inbound Rules
- **Action**: Edit the inbound rules of `my-ssh-security-group` to allow SSH traffic.
- **Rule Added**: Allow SSH from anywhere (0.0.0.0/0).
- **Result**: After saving the rules, the connection attempt succeeds.

## Installing Software
### Step 7: Check Software Installation
- **Command**: `service httpd status`
- **Result**: The HTTPD service is not found because the installation script did not execute successfully.

### Reason for Installation Failure
- **Explanation**: The outbound rules were not set, preventing the instance from reaching the internet to download the HTTPD package.

### Step 8: Modify Outbound Rules
- **Action**: Add outbound rules to allow HTTP and HTTPS traffic in `my-web-security-group`.
- **Rules Added**:
  - Allow HTTP (port 80) to anywhere (0.0.0.0/0).
  - Allow HTTPS (port 443) to anywhere (0.0.0.0/0).

### Step 9: Attach Web Security Group
- **Action**: Change the security group of the EC2 instance to include `my-web-security-group`.
- **Result**: The instance can now access the internet to install the HTTPD package.

## Verifying Installation
### Step 10: Re-attempt Software Installation
- **Command**: `yum install httpd -y`
- **Result**: The installation succeeds, and the HTTPD service can be started.

### Step 11: Create HTML File
- **Command**: `echo "Security Groups Test" > /var/www/html/index.html`
- **Purpose**: Create a test HTML file to verify web server functionality.

## Accessing the Web Server
### Step 12: Access from Browser
- **Action**: Use the public IP of the EC2 instance to access the web server.
- **Result**: Initially fails due to lack of inbound HTTP rules.

### Step 13: Modify Inbound Rules for Web Security Group
- **Action**: Add inbound rule to allow HTTP traffic in `my-web-security-group`.
- **Rule Added**: Allow HTTP (port 80) from anywhere (0.0.0.0/0).
- **Result**: After saving the rules, the web page is accessible.

## Understanding Security Group Behavior
### Key Points
- **Stateful Nature**: Security groups are stateful; if an outbound request is allowed, the corresponding inbound response is automatically allowed.
- **Allow Only Rules**: Security groups only allow traffic that is explicitly defined; any traffic not defined is denied.
- **Multiple Security Groups**: An EC2 instance can have multiple security groups, and all rules from attached security groups are evaluated together.

## Elastic Network Interfaces (ENIs)
### Overview
- **Definition : Elastic Network Interfaces (ENIs) are virtual network interfaces that allow EC2 instances to connect to the network.
- **Components**: Each ENI has a public IP, private IP, and can be associated with multiple security groups.

### Benefits of ENIs
- **Flexibility**: ENIs can be detached from one EC2 instance and attached to another, allowing for high availability and failover scenarios.
- **Multiple IPs**: Each ENI can have multiple private IPs and one public IP, enabling different applications to run on the same instance with separate network interfaces.

### Managing ENIs
- **Creation**: ENIs can be created separately and attached to EC2 instances as needed.
- **Configuration**: When creating an ENI, you can specify the subnet and security groups to associate with it.

## Conclusion
- **Security Groups**: Understanding how to configure security groups is crucial for managing access to EC2 instances and ensuring proper functionality of applications.
- **ENIs**: Utilizing ENIs provides additional flexibility in network management and can enhance the resilience of your applications.
- **Next Steps**: After the break, the discussion will continue with Network ACLs, which provide an additional layer of security at the subnet level.
