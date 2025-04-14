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

### 4. **Route 53 Configuration**:
   - Set up a **Route 53 hosted zone** and direct traffic to the **EC2 instance**.

## Additional Requirements:
### 1. **Testing Automation for the Development Team**:
   - Provide a solution that allows the **development team** to test their code without needing system admins.
   - The solution should allow the development team to focus on testing rather than provisioning, configuring, and updating the resources needed for testing.

### 2. **RDS Instance Retention**:
   - Ensure that when the development team deletes the stack, the **RDS DB instances** are **not deleted**.

---

Here's a complete breakdown and solution for the **Case Study – Multi-Tier Architecture** using **AWS CloudFormation**, which automates the provisioning and configuration of infrastructure, empowering your development team to test independently of system admins.

---

### 🧩 **Architecture Overview**

**1. Web Tier (Public Subnet)**  
**2. App Tier (Private Subnet)**  
**3. DB Tier (Private Subnet + RDS)**  
**4. Route 53 for DNS Routing**  
**5. Automation with CloudFormation**  
**6. RDS Retention after Stack Deletion**

---

### ✅ **CloudFormation Template Highlights**

We’ll split this into functional blocks:

---

### **1. VPC, Subnets, and Routing**
Create a custom VPC with:

- 1 public subnet (for Web Tier)
- 1 private subnet (for App Tier)
- 1 private subnet (for DB Tier)
- Internet Gateway for public access
- NAT Gateway (optional but helpful for outbound internet from private tiers)

---

### **2. Security Groups**

- **Web SG**: Allow SSH (port 22) and HTTP (port 80) from anywhere (for testing only)
- **App SG**: Allow SSH only from Web Tier subnet IP range
- **DB SG**: Allow MySQL (port 3306) only from App Tier subnet

---

### **3. EC2 Instances and RDS**

- **Web EC2 Instance**: Public IP enabled, launched in Public Subnet
- **App EC2 Instance**: No public IP, launched in Private Subnet
- **RDS MySQL**: DeletionPolicy set to **Retain**, launched in DB Subnet

---

### **4. Route 53 Hosted Zone**

- Create a Route 53 hosted zone
- Associate a DNS record pointing to the Web EC2 public IP

---

### **5. Template Snippet – Core CloudFormation Sections**

Below is a simplified version of the **CloudFormation YAML template**. The full version would include Outputs, Parameters, IAM roles, NAT gateway setup, etc.

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Multi-Tier Architecture with Web, App, DB tiers, Route 53, and RDS Retention

Resources:

  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true

  InternetGateway:
    Type: AWS::EC2::InternetGateway

  AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref VPC
      InternetGatewayId: !Ref InternetGateway

  PublicSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      CidrBlock: 10.0.1.0/24
      VpcId: !Ref VPC
      MapPublicIpOnLaunch: true

  PrivateAppSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      CidrBlock: 10.0.2.0/24
      VpcId: !Ref VPC

  PrivateDBSubnetA:
    Type: AWS::EC2::Subnet
    Properties:
      CidrBlock: 10.0.3.0/24
      VpcId: !Ref VPC
      AvailabilityZone: us-east-1a
  
  PrivateDBSubnetB:
    Type: AWS::EC2::Subnet
    Properties:
      CidrBlock: 10.0.4.0/24
      VpcId: !Ref VPC
      AvailabilityZone: us-east-1b

  WebSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Web Tier SG
      VpcId: !Ref VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

  AppSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: App Tier SG
      VpcId: !Ref VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 10.0.1.0/24  # Only Web Tier

  DBSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: DB Tier SG
      VpcId: !Ref VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 3306
          ToPort: 3306
          CidrIp: 10.0.2.0/24  # Only App Tier

  WebInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-00a929b66ed6e0de6  # Update to your region
      SubnetId: !Ref PublicSubnet
      SecurityGroupIds: [!Ref WebSecurityGroup]

  AppInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-00a929b66ed6e0de6
      SubnetId: !Ref PrivateAppSubnet
      SecurityGroupIds: [!Ref AppSecurityGroup]

  MyRDS:
    Type: AWS::RDS::DBInstance
    DeletionPolicy: Retain  # Ensures DB is NOT deleted with stack
    Properties:
      DBInstanceIdentifier: dev-db
      AllocatedStorage: 20
      DBInstanceClass: db.t3.micro
      Engine: MySQL
      EngineVersion: 8.0.40
      MasterUsername: admin
      MasterUserPassword: MySecurePassword123
      VPCSecurityGroups: [!Ref DBSecurityGroup]
      DBSubnetGroupName: !Ref MyDBSubnetGroup
      PubliclyAccessible: false
      MultiAZ: false

  MyDBSubnetGroup:
    Type: AWS::RDS::DBSubnetGroup
    DependsOn: MyRDS
    Properties:
      DBSubnetGroupDescription: Subnet group for RDS
      SubnetIds:
        - !Ref PrivateDBSubnetA
        - !Ref PrivateDBSubnetB

  HostedZone:
    Type: AWS::Route53::HostedZone
    Properties:
      Name: yourcompany.dev.

  WebDNSRecord:
    Type: AWS::Route53::RecordSet
    Properties:
      HostedZoneName: yourcompany.dev.
      Name: web.yourcompany.dev.
      Type: A
      TTL: 300
      ResourceRecords:
        - !GetAtt WebInstance.PublicIp
```

---

### 🧪 **Automation for Developers**

To allow developers to test without involving system admins:

- **CloudFormation StackSets** or **Self-service Portals (like AWS Service Catalog)** can be used
- CI/CD integration with CodePipeline and CodeBuild
- Provide developers a **predefined CloudFormation template** so they can launch the test environment with one click via console or CLI
- Use **Parameter Store** or **Secrets Manager** to handle credentials

---

### 🎯 Summary

✅ Multi-tier AWS architecture  
✅ Public Web EC2 with HTTP/SSH  
✅ Private App EC2 with restricted SSH  
✅ RDS in a private subnet with retained deletion policy  
✅ Route 53 DNS setup  
✅ Developer self-service via CloudFormation automation

---
