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

To address the problem statement and the tasks listed, we can implement a solution leveraging **AWS CloudFormation** for automation, along with **Amazon EC2**, **Amazon RDS**, **Amazon Route 53**, and other services. The solution will also incorporate **IAM roles** and **security groups** to handle access and permissions. Below is a step-by-step solution breakdown.

---

### **Solution Overview**

1. **Web Tier (Public EC2 Instance)**
    - Launch an EC2 instance in a **public subnet**.
    - This EC2 instance will have a **security group** that allows **HTTP (port 80)** and **SSH (port 22)** from the internet.

2. **Application Tier (Private EC2 Instance)**
    - Launch an EC2 instance in a **private subnet** (not directly accessible from the internet).
    - This EC2 instance will allow only **SSH access from the public subnet** of the Web Tier.

3. **DB Tier (RDS MySQL Instance)**
    - Launch an **RDS MySQL instance** in a **private subnet**.
    - The RDS instance will allow connections only from the **private subnet** of the Application Tier on **port 3306**.

4. **Route 53 Configuration**
    - Set up a **Route 53 hosted zone** and create a DNS record to route traffic to the EC2 instance in the Web Tier.

5. **Testing Automation**
    - **CloudFormation templates** will be used to provision, configure, and update the infrastructure automatically for the development team.
    - The team will only need to trigger the CloudFormation stack creation for the testing environment without involving system admins.

6. **RDS Instance Retention**
    - To ensure the RDS instance is retained after the stack is deleted, we'll use the `DeletionPolicy` attribute in the CloudFormation template to **retain** the RDS instance when the stack is deleted.

---

### **CloudFormation Template Solution**

Below is an example **CloudFormation template** in YAML format that will deploy the described resources:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: Multi-Tier Web Application with Testing Environment

Parameters:
  KeyName:
    Description: EC2 Key Pair for SSH access
    Type: AWS::EC2::KeyPair::KeyName

Resources:

  # 1. Web Tier - Public EC2 Instance
  WebInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c55b159cbfafe1f0  # Example AMI ID for Amazon Linux 2 (ensure to use the correct AMI for your region)
      SubnetId: !Ref PublicSubnetId
      SecurityGroupIds:
        - !Ref WebSecurityGroup
      KeyName: !Ref KeyName
      Tags:
        - Key: Name
          Value: WebTier-Instance

  WebSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow HTTP and SSH from the internet
      VpcId: !Ref VPCId
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: "80"
          ToPort: "80"
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: "22"
          ToPort: "22"
          CidrIp: 0.0.0.0/0

  # 2. Application Tier - Private EC2 Instance
  AppInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c55b159cbfafe1f0  # Example AMI ID for Amazon Linux 2
      SubnetId: !Ref PrivateSubnetId
      SecurityGroupIds:
        - !Ref AppSecurityGroup
      KeyName: !Ref KeyName
      Tags:
        - Key: Name
          Value: AppTier-Instance

  AppSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow SSH from the Web Tier subnet
      VpcId: !Ref VPCId
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: "22"
          ToPort: "22"
          CidrIp: !GetAtt WebInstance.PrivateIp

  # 3. DB Tier - RDS MySQL Instance
  MySQLDB:
    Type: AWS::RDS::DBInstance
    Properties:
      DBInstanceIdentifier: mydb-instance
      AllocatedStorage: 20
      DBInstanceClass: db.t2.micro
      Engine: MySQL
      EngineVersion: "8.0"
      MasterUsername: admin
      MasterUserPassword: password123
      DBName: testdb
      VPCSecurityGroups:
        - !Ref MySQLSecurityGroup
      DBSubnetGroupName: !Ref DBSubnetGroup
      DeletionProtection: true
      DeletionPolicy: Retain  # Prevent deletion of RDS when stack is deleted

  MySQLSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow MySQL access from App Tier
      VpcId: !Ref VPCId
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: "3306"
          ToPort: "3306"
          CidrIp: !GetAtt AppInstance.PrivateIp

  DBSubnetGroup:
    Type: AWS::RDS::DBSubnetGroup
    Properties:
      DBSubnetGroupDescription: RDS Subnet Group
      SubnetIds:
        - !Ref PrivateSubnetId

  # 4. Route 53 Configuration - Hosted Zone and Record Set
  HostedZone:
    Type: AWS::Route53::HostedZone
    Properties:
      Name: example.com

  DNSRecordSet:
    Type: AWS::Route53::RecordSet
    Properties:
      HostedZoneId: !Ref HostedZone
      Name: app.example.com
      Type: A
      TTL: 60
      ResourceRecords:
        - !GetAtt WebInstance.PublicIp

Outputs:
  WebInstancePublicIP:
    Description: Public IP of Web Tier EC2
    Value: !GetAtt WebInstance.PublicIp

  DBInstanceEndpoint:
    Description: Endpoint of MySQL DB
    Value: !GetAtt MySQLDB.Endpoint.Address
```

---

### **Explanation of Key Components**:

1. **Web Instance (Public EC2)**:
   - **Security Group**: Allows **HTTP (80)** and **SSH (22)** from the internet (0.0.0.0/0).
   - **Subnet**: Launched in a **public subnet** with internet access.

2. **App Instance (Private EC2)**:
   - **Security Group**: Allows **SSH (22)** from the **public subnet**.
   - **Subnet**: Launched in a **private subnet** with no direct internet access.

3. **RDS MySQL Instance**:
   - Launched in a **private subnet** and allows connections only from the **App Instance** on port 3306.
   - **DeletionPolicy**: Set to **Retain** to prevent deletion when the stack is removed.

4. **Route 53**:
   - A **hosted zone** is created for the domain `example.com`, and a DNS record (`A`) points to the **Web Instance**'s public IP address.

5. **Testing Automation**:
   - CloudFormation handles the creation, updating, and deletion of resources, so the development team can focus on testing without managing infrastructure.

---

### **Steps for the Development Team**:

1. **Deploy the Stack**: The development team uses the provided CloudFormation template to deploy the infrastructure automatically.
2. **Test the Application**: Once the stack is deployed, the team can test their code on the public EC2 instance without worrying about provisioning resources.
3. **Cleanup**: When testing is complete, the team deletes the stack, but the RDS instance is retained (due to the `DeletionPolicy: Retain`), preserving the database.

This solution ensures the infrastructure is fully automated and does not require system admin intervention during the testing phase.
