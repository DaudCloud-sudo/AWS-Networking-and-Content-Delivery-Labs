# AWS Networking and Content Delivery Lab

# AWS VPC Setup with Security Enhancements

This lab focuses on setting up a Virtual Private Cloud (VPC) in AWS, highlighting its features and security aspects. The setup includes creating a new VPC, adding public and private subnets, configuring route tables, and implementing security measures with Network ACLs and Security Groups. This guide excludes the EC2 instance setup; for detailed EC2 setup instructions, please refer to [AWS Compute Labs](https://github.com/DaudCloud-sudo/AWS-Compute-Labs).

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Task 1: Creating the VPC](#task-1-creating-the-vpc)
- [Task 2: Creating Subnets](#task-2-creating-subnets)
- [Task 3: Creating Route Tables](#task-3-creating-route-tables)
- [Task 4: Configuring Network ACLs](#task-4-configuring-network-acls)
- [Task 5: Configuring Security Groups](#task-5-configuring-security-groups)
- [Understanding VPN Concepts](#understanding-vpn-concepts)
- [Resources](#resources)

## Introduction

In this exercise, you will set up a new VPC in AWS to understand its core components and security features. This new VPC will have:
- Two public subnets
- Two private subnets
- A public route table
- A private route table

  ![image](https://github.com/user-attachments/assets/a23223fd-d958-4fc3-887b-8f3306ccec8a)

Additionally, you'll learn to configure Network ACLs and Security Groups to secure your application infrastructure.

## Prerequisites

- An AWS account with appropriate permissions to create VPCs, subnets, and route tables.
- Basic knowledge of AWS services and networking concepts.

## Task 1: Creating the VPC

1. **Log in** to the AWS Management Console as your Admin user.
2. In the **Services** search box, enter **VPC** and open the VPC console.
3. In the navigation pane, under **Virtual Private Cloud**, choose **Your VPCs**.
4. Choose **Create VPC**.
5. Configure the following settings:
   - **Name tag:** `app-vpc`
   - **IPv4 CIDR block:** `10.1.0.0/16`
6. Choose **Create VPC**.

![image](https://github.com/user-attachments/assets/a544a316-1aa6-433e-8558-931bfb7c97f2)

7. In the navigation pane, under **Virtual private cloud**, choose **Internet Gateways**.
8. Choose **Create internet gateway**.
9. For **Name tag**, enter `app-igw` and choose **Create internet gateway**.
10. In the details page for the internet gateway, choose **Actions** and then **Attach to VPC**.
11. For **Available VPCs**, choose `app-vpc` and then choose **Attach internet gateway**.

![image](https://github.com/user-attachments/assets/695cfe38-b878-403b-bc28-7a3b14da858f)

## Task 2: Creating Subnets

1. In the navigation pane, choose **Subnets**.
2. Choose **Create subnet**.
3. Configure the subnets as follows:

   - **Public Subnet 1**:
     - **VPC ID:** `app-vpc`
     - **Subnet name:** `Public Subnet 1`
     - **Availability Zone:** Choose the first Availability Zone (e.g., `us-east-2a`)
     - **IPv4 CIDR block:** `10.1.1.0/24`

   - **Public Subnet 2**:
     - **Subnet name:** `Public Subnet 2`
     - **Availability Zone:** Choose the second Availability Zone (e.g., `us-east-2b`)
     - **IPv4 CIDR block:** `10.1.2.0/24`

   - **Private Subnet 1**:
     - **Subnet name:** `Private Subnet 1`
     - **Availability Zone:** Choose the first Availability Zone (e.g., `us-east-2a`)
     - **IPv4 CIDR block:** `10.1.3.0/24`

   - **Private Subnet 2**:
     - **Subnet name:** `Private Subnet 2`
     - **Availability Zone:** Choose the second Availability Zone (e.g., `us-east-2b`)
     - **IPv4 CIDR block:** `10.1.4.0/24`

4. Choose **Create subnet**.

![image](https://github.com/user-attachments/assets/8c92a834-41a7-40b4-8447-957939f7d138)

## Task 3: Creating Route Tables

1. In the navigation pane, choose **Route Tables**.
2. Choose **Create route table**.
3. Configure the public route table:
   - **Name:** `app-routetable-public`
   - **VPC:** `app-vpc`
4. Choose **Create route table**.
5. Choose **app-routetable-public** from the list, then go to the **Routes** tab and choose **Edit routes**.
6. Add a route:
   - **Destination:** `0.0.0.0/0`
   - **Target:** Choose **Internet Gateway** and then select `app-igw`.
7. Choose **Save changes**.
8. Go to the **Subnet associations** tab, choose **Edit subnet associations**, select both public subnets, and choose **Save associations**.

![image](https://github.com/user-attachments/assets/9eb2f0df-c865-4963-b2fa-1465aa55577d)

10. Repeat the above steps to create a private route table:
   - **Name:** `app-routetable-private`
   - **VPC:** `app-vpc`

11. Associate the private route table with both private subnets.

![image](https://github.com/user-attachments/assets/7db11f1c-5f11-42b2-ae42-f77d887bea4f)

## Task 4: Configuring Network ACLs

1. In the VPC console, navigate to **Network ACLs**.
2. Choose **Create Network ACL**.
3. Configure the following settings:
   - **Name tag:** `app-acl-public`
   - **VPC:** `app-vpc`
4. Choose **Create**.
5. Select the **app-acl-public** and choose **Inbound rules**.
6. Choose **Edit inbound rules** and add rules to allow HTTP (port 80), HTTPS (port 443), and SSH (port 22) traffic.
7. Configure **Outbound rules** to allow all traffic (0.0.0.0/0).
8. Associate the Public Subnets with it.
9. Repeat steps 2-7 to create a private network ACL (`app-acl-private`) and configure appropriate inbound and outbound rules.

![image](https://github.com/user-attachments/assets/18611de6-bd87-43a4-a32e-4f16262c263b)

## Task 5: Configuring Security Groups

1. In the VPC console, navigate to **Security Groups**.
2. Choose **Create security group**.
3. Configure the following settings:
   - **Security group name:** `web-security-group`
   - **Description:** `Enable HTTP and HTTPS access`
   - **VPC:** `app-vpc`
4. Add the following **Inbound rules**:
   - **Type:** HTTP, **Source:** Anywhere (0.0.0.0/0)
   - **Type:** HTTPS, **Source:** Anywhere (0.0.0.0/0)
5. Ensure that the **Outbound rules** allow all traffic (0.0.0.0/0).
6. Choose **Create security group**.

![image](https://github.com/user-attachments/assets/71412fd3-ecbe-4baa-b05a-cf36e43de819)

For further details on setting up EC2 instances, refer to [AWS Compute Labs](https://github.com/DaudCloud-sudo/AWS-Compute-Labs).

## Understanding VPN Concepts

### Customer Gateway (CGW)
A **Customer Gateway** is a resource that you create in AWS, representing your physical network's gateway device. It is used to establish a secure connection between your on-premises network and your AWS VPC.

### Virtual Private Gateway (VPG)
A **Virtual Private Gateway (VPG)** is a logical device in AWS that enables a secure connection from a VPC to an on-premises network. It acts as a VPN concentrator on the Amazon side of the VPN connection.

### Site-to-Site VPN
A **Site-to-Site VPN** creates a secure, encrypted connection between your on-premises network and your AWS VPC. This type of VPN is commonly used to extend your on-premises network to AWS securely. The connection is established between your Customer Gateway and AWS's Virtual Private Gateway.

### Client VPN
An **AWS Client VPN** is a managed client-based VPN service that allows secure access to your AWS resources and on-premises networks. With Client VPN, users connect securely to your AWS network from any location using OpenVPN-based clients.

## Resources

- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [AWS Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
- [AWS Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
- [AWS Site-to-Site VPN](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPN_Overview.html)
- [AWS Client VPN](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/what-is.html)

  
**Note:** This guide is based on the AWS SAA preparation course lab. Ensure you terminate all resources to avoid incurring costs.
