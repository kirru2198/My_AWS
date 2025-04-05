# CloudFormation VPC Template

## Problem Statement:
You work for XYZ Corporation. Your team is tasked with deploying a similar architecture multiple times for testing, development, and production purposes. Implement CloudFormation for the tasks assigned to you below.

## Tasks to Be Performed:
1. Create a template with 1 VPC and 1 public subnet.
2. Launch an Amazon Linux EC2 instance in the public subnet and tag the instance as "CFinstance".
---
To meet the requirements mentioned in the problem statement, we'll create a CloudFormation template that:

1. Creates a Virtual Private Cloud (VPC).
2. Creates a public subnet inside the VPC.
3. Launches an Amazon Linux EC2 instance in the public subnet.
4. Tags the EC2 instance with the name "CFinstance".

Here is a basic CloudFormation template in YAML format to achieve this:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: CloudFormation Template for deploying VPC, Subnet, and EC2 instance

Resources:
  # Create the VPC
  MyVPC:
    Type: 'AWS::EC2::VPC'
    Properties:
      CidrBlock: '10.0.0.0/16'
      EnableDnsSupport: 'true'
      EnableDnsHostnames: 'true'
      Tags:
        - Key: Name
          Value: 'MyVPC'

  # Create the public subnet within the VPC
  MyPublicSubnet:
    Type: 'AWS::EC2::Subnet'
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: '10.0.1.0/24'
      AvailabilityZone: 'us-east-1a'  # Replace with your desired AZ
      MapPublicIpOnLaunch: 'true'
      Tags:
        - Key: Name
          Value: 'MyPublicSubnet'

  # Create an Internet Gateway to allow internet access to the VPC
  MyInternetGateway:
    Type: 'AWS::EC2::InternetGateway'
    Properties:
      Tags:
        - Key: Name
          Value: 'MyInternetGateway'

  # Attach the Internet Gateway to the VPC
  AttachGateway:
    Type: 'AWS::EC2::VPCGatewayAttachment'
    Properties:
      VpcId: !Ref MyVPC
      InternetGatewayId: !Ref MyInternetGateway

  # Create an Amazon Linux EC2 instance
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: 't2.micro'  # You can change the instance type as required
      ImageId: 'ami-0c55b159cbfafe1f0'  # Amazon Linux 2 AMI (make sure to update this with the correct one for your region)
      SubnetId: !Ref MyPublicSubnet
      Tags:
        - Key: Name
          Value: 'CFinstance'

Outputs:
  VPCId:
    Description: 'VPC ID'
    Value: !Ref MyVPC

  SubnetId:
    Description: 'Subnet ID'
    Value: !Ref MyPublicSubnet

  EC2InstanceId:
    Description: 'EC2 Instance ID'
    Value: !Ref MyEC2Instance
```

### Explanation of Key Components:
- **VPC (`MyVPC`)**: Creates a Virtual Private Cloud with a CIDR block of `10.0.0.0/16`. DNS support is enabled for public DNS resolution.
  
- **Public Subnet (`MyPublicSubnet`)**: Creates a public subnet within the VPC with a CIDR block of `10.0.1.0/24`. It is placed in Availability Zone `us-east-1a`. The `MapPublicIpOnLaunch` property ensures that any instances launched in this subnet will automatically receive a public IP address.

- **Internet Gateway (`MyInternetGateway`)**: An Internet Gateway is created and attached to the VPC to allow instances in the public subnet to access the internet.

- **EC2 Instance (`MyEC2Instance`)**: An Amazon Linux EC2 instance is launched in the public subnet (`MyPublicSubnet`). It is tagged as "CFinstance". The AMI ID used here is for the Amazon Linux 2 image; you should verify this AMI ID based on your region.

### Outputs:
- The template outputs the IDs of the VPC, Subnet, and EC2 instance created, which can be useful for debugging or subsequent steps.

### Notes:
1. **ImageId**: The `ImageId` (`ami-0c55b159cbfafe1f0`) is specific to Amazon Linux 2 and is valid in certain regions. Please make sure to use the correct AMI ID for your region. You can find the AMI ID by searching the AWS Marketplace or using the AWS CLI for your region.
   
2. **Availability Zone**: `us-east-1a` is used as an example for the Availability Zone. You can change it according to the region where you're deploying.

3. **Instance Type**: The instance type is set to `t2.micro`, which is eligible for the AWS free tier. You can modify it as needed.

4. **Public IP Assignment**: The EC2 instance launched in the public subnet will automatically receive a public IP address due to the `MapPublicIpOnLaunch` property set to `true`.

This template should allow you to deploy the requested infrastructure in AWS. Let me know if you need any adjustments or further details!
