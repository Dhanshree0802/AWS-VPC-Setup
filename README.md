# AWS VPC Setup with Public and Private Subnets

## Project Overview
This project demonstrates the creation of a Virtual Private Cloud (VPC) in AWS, complete with public and private subnets. It involves configuring route tables, setting up security groups, and launching a Windows EC2 instance in the public subnet. The project aims to provide a secure and scalable environment for cloud resources, highlighting key concepts in AWS networking.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Project Steps](#project-steps)
  - [1. Create a Virtual Private Cloud (VPC)](#1-create-a-virtual-private-cloud-vpc)
  - [2. Create Subnets](#2-create-subnets)
  - [3. Create an Internet Gateway](#3-create-an-internet-gateway)
  - [4. Configure Route Tables](#4-configure-route-tables)
  - [5. Configure Security Groups](#5-configure-security-groups)
  - [6. Launch a Windows EC2 Instance in the Public Subnet](#6-launch-a-windows-ec2-instance-in-the-public-subnet)
- [Summary](#summary)

## Prerequisites
- An AWS account
- Basic understanding of AWS services, particularly VPC and EC2
- Familiarity with the AWS Management Console

## Project Steps

### 1. Create a Virtual Private Cloud (VPC)
1. Navigate to the AWS VPC Dashboard.
2. Click on **Create VPC** and select **VPC and more**.
3. Enter a name for your VPC (e.g., `MyVPC`).
4. Set the IPv4 CIDR block to `10.0.0.0/16`.
5. Leave the IPv6 CIDR block empty.
6. Set the Tenancy to `default`.
7. Click **Create VPC**.

### 2. Create Subnets
#### Create a Public Subnet
1. Go to the **Subnets** section in the VPC dashboard.
2. Click **Create Subnet**.
3. Name the subnet `Public-Subnet`.
4. Select your VPC (`MyVPC`) and an Availability Zone (e.g., `us-east-1a`).
5. Set the IPv4 CIDR block to `10.0.1.0/24`.
6. Click **Create Subnet**.

#### Create a Private Subnet
1. Repeat the steps to create another subnet.
2. Name it `Private-Subnet`.
3. Set the IPv4 CIDR block to `10.0.2.0/24`.
4. Click **Create Subnet**.

### 3. Create an Internet Gateway
1. In the VPC dashboard, go to **Internet Gateways**.
2. Click **Create Internet Gateway**.
3. Name it `MyInternetGateway`.
4. Click **Create Internet Gateway**.
5. Attach the Internet Gateway to your VPC (`MyVPC`).

### 4. Configure Route Tables
#### Public Route Table
1. Go to **Route Tables** in the VPC dashboard.
2. Click **Create Route Table**.
3. Name it `Public-Route-Table` and select your VPC.
4. Add a route:
   - **Destination**: `0.0.0.0/0`
   - **Target**: The Internet Gateway (`MyInternetGateway`)
5. Associate the public subnet (`Public-Subnet`) with this route table.

#### Private Route Table
1. Create another route table named `Private-Route-Table`.
2. Associate the private subnet (`Private-Subnet`) with this route table.

### 5. Configure Security Groups
#### Public Security Group
1. Go to **Security Groups** in the VPC dashboard.
2. Create a new security group named `Public-SG` for your VPC.
3. Add inbound rules for:
   - **HTTP (80)**: Allow from anywhere.
   - **HTTPS (443)**: Allow from anywhere.
   - **RDP (3389)**: Allow from your IP.
4. Allow all outbound traffic.

#### Private Security Group (for accessing this private group we need Nat-gateway which is chargeable, so I just have mentioned the steps in case if you want to launch)
1. Create another security group named `Private-SG`.
2. Add inbound rules for database traffic (e.g., MySQL port 3306) from `Public-SG`.
3. Allow all outbound traffic.

### 6. Launch a Windows EC2 Instance in the Public Subnet
1. Go to the EC2 dashboard and click **Launch Instance**.
2. Choose a Windows AMI (e.g., "Microsoft Windows Server 2019 Base").
3. Select `t2.micro` as the instance type.
4. Configure the instance:
   - Select your VPC (`MyVPC`).
   - Choose the public subnet (`Public-Subnet`).
   - Enable "Auto-assign Public IP".
5. Attach the `Public-SG` security group.
6. Review and launch the instance.
7. Download the `.pem` file to access the instance via RDP.

## Summary
This project successfully demonstrates the creation of a Virtual Private Cloud (VPC) with public and private subnets in AWS, including the configuration of route tables and security groups, and the deployment of a Windows EC2 instance. This foundational setup is essential for building secure and scalable cloud infrastructures in AWS.
