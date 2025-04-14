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

To meet the requirements outlined in this case study, we can use **AWS CloudFormation** to automate the provisioning of infrastructure and ensure consistent deployments. Below is a breakdown of the **multi-tier architecture solution**, including:

- Networking setup
- EC2 and RDS provisioning
- Route 53 configuration
- Security settings
- Automation and RDS retention

---

## ✅ **Solution Outline**

---

### 🔹 **1. VPC and Subnet Setup**

Create a VPC with public and private subnets across two Availability Zones (AZs) for high availability:

- **Public Subnet** (for Web Tier):
  - Route to Internet Gateway
- **Private Subnet for App Tier**:
  - No internet access, can access the Web Tier
- **Private Subnet for DB Tier**:
  - No internet access, only accessible by App Tier

---

### 🔹 **2. EC2 Instances**

#### Web Tier (Public EC2 Instance):
- Launch EC2 instance in the **public subnet**
- Allow **HTTP (80)** and **SSH (22)** from the internet via a **Security Group**
  
#### Application Tier (Private EC2 Instance):
- Launch EC2 instance in the **private subnet**
- Allow **SSH** only from **Web Tier's security group**

---

### 🔹 **3. RDS MySQL (DB Tier)**

- Launch RDS MySQL in **private subnet**
- Set **deletion policy to "Retain"** to prevent data loss when the stack is deleted
- Allow connections only from **App Tier subnet** on **port 3306**

---

### 🔹 **4. Route 53 Configuration**

- Create a **Route 53 Hosted Zone**
- Create a **record set** pointing to the **public IP or Elastic IP** of the Web Tier EC2

---

### 🔹 **5. Testing Automation for Development Team**

Use **CloudFormation Templates** and possibly **AWS Service Catalog** to:

- Allow dev team to launch testing environments quickly without sysadmin help
- Include **user data scripts** to install web servers or application code
- Use **Auto Scaling groups or EC2 Image Builder** if scaling or image consistency is required

---

### 🔹 **6. CloudFormation Template Features**

- **Parameters** for customization (instance types, DB names, passwords)
- **Security Groups** configured per tier
- **Retention Policy** for RDS:
  ```yaml
  DeletionPolicy: Retain
  ```
- **Outputs** for public DNS, RDS endpoint, etc.

---

## 🛠️ CloudFormation Snippet (Example)

```yaml
Resources:

  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: MyVPC

  PublicSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: 10.0.1.0/24
      MapPublicIpOnLaunch: true
      AvailabilityZone: !Select [ 0, !GetAZs '' ]

  PrivateSubnetApp:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: 10.0.2.0/24
      AvailabilityZone: !Select [ 0, !GetAZs '' ]

  PrivateSubnetDB:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: 10.0.3.0/24
      AvailabilityZone: !Select [ 1, !GetAZs '' ]

  WebInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0abcdef1234567890
      SubnetId: !Ref PublicSubnet
      SecurityGroupIds: [!Ref WebSG]
      KeyName: my-key
      Tags:
        - Key: Name
          Value: WebServer

  AppInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0abcdef1234567890
      SubnetId: !Ref PrivateSubnetApp
      SecurityGroupIds: [!Ref AppSG]
      KeyName: my-key
      Tags:
        - Key: Name
          Value: AppServer

  MyRDS:
    Type: AWS::RDS::DBInstance
    DeletionPolicy: Retain
    Properties:
      DBName: mydb
      AllocatedStorage: 20
      DBInstanceClass: db.t3.micro
      Engine: mysql
      MasterUsername: admin
      MasterUserPassword: mypassword
      VPCSecurityGroups: [!Ref DBSG]
      DBSubnetGroupName: !Ref MyDBSubnetGroup

  MyDBSubnetGroup:
    Type: AWS::RDS::DBSubnetGroup
    Properties:
      DBSubnetGroupDescription: "DB subnet group"
      SubnetIds:
        - !Ref PrivateSubnetDB

Outputs:
  WebPublicIP:
    Value: !GetAtt WebInstance.PublicIp
    Description: Public IP of Web Server
```

---

## 🧪 Testing Automation Flow

1. Dev team launches CloudFormation stack via **AWS Console** or **CLI**
2. EC2 instances auto-configure via **user-data**
3. Route 53 updates DNS to Web Server
4. Devs test via browser or tools
5. Upon stack deletion, all infra is deleted **except RDS**

---
