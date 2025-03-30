# aws-networking

# AWS Networking Setup Guide

## Overview
This guide walks you through setting up a **Virtual Private Cloud (VPC)** with subnets across multiple **Availability Zones**, configuring a **NAT Gateway** for private instances to access the internet, and implementing **Security Groups** and **Network ACLs (NACLs)** to control traffic.

## Prerequisites
- An AWS account with necessary permissions.
- Access to the AWS Management Console.

## Steps to Setup

### Create a VPC
1. Navigate to **AWS Console > VPC**.
2. Click **Create VPC**.
3. Choose **VPC only**.
4. Enter a **Name tag** (e.g., `MyVPC`).
5. Set an IPv4 CIDR block (e.g., `10.0.0.0/16`).
6. Click **Create VPC**.

### Create Subnets in Different Availability Zones
1. Go to **Subnets** > **Create subnet**.
2. Select the VPC created earlier.
3. Create two subnets in different availability zones:
   - **Subnet 1 (Public):**
     - Name: `PublicSubnet`
     - CIDR: `10.0.1.0/24`
     - Availability Zone: Choose one
   - **Subnet 2 (Private):**
     - Name: `PrivateSubnet`
     - CIDR: `10.0.2.0/24`
     - Availability Zone: Choose a different one
4. Click **Create subnet**.

### Create an Internet Gateway
1. Go to **Internet Gateways** > **Create internet gateway**.
2. Enter **Name tag** (e.g., `MyIGW`).
3. Click **Create Internet Gateway**.
4. Attach it to your VPC: Select the IGW > **Actions** > **Attach to VPC** > Choose `MyVPC` > **Attach**.

### Configure Route Tables
1. **Public Route Table:**
   - Go to **Route Tables** > **Create route table**.
   - Name it `PublicRouteTable` and attach it to `MyVPC`.
   - Add a route: Destination `0.0.0.0/0`, Target: **Internet Gateway** (`MyIGW`).
   - Associate `PublicSubnet` with this route table.

2. **Private Route Table:**
   - Create a new route table `PrivateRouteTable`.
   - Associate `PrivateSubnet` with this route table (no internet access yet).

### Create a NAT Gateway
1. Go to **NAT Gateways** > **Create NAT Gateway**.
2. Select `PublicSubnet`.
3. Allocate a new **Elastic IP**.
4. Click **Create NAT Gateway**.
5. Update `PrivateRouteTable`:
   - Add a route: Destination `0.0.0.0/0`, Target: **NAT Gateway**.

### Configure Security Groups
1. Go to **Security Groups** > **Create security group**.
2. **Public Instances SG:**
   - Allow **Inbound:** HTTP (80), HTTPS (443), SSH (22), and custom ports.
   - Allow **Outbound:** All traffic.
3. **Private Instances SG:**
   - Allow **Inbound:** Only SSH (22) from `PublicSubnet`.
   - Allow **Outbound:** All traffic.

### Set Up Network ACLs (NACLs)
1. Go to **Network ACLs** > **Create NACL**.
2. **PublicSubnet NACL:**
   - Allow inbound: HTTP (80), HTTPS (443), SSH (22) from all sources.
   - Allow outbound: All traffic.
3. **PrivateSubnet NACL:**
   - Allow inbound: All traffic from `PublicSubnet`.
   - Allow outbound: All traffic to `PublicSubnet`.


SCREENSHOTS
![VPC](https://github.com/Nifesimi-p/aws-networking/blob/main/VPC.png)
![SUBNETS](https://github.com/Nifesimi-p/aws-networking/blob/main/SUBNETS.png)
![NACL](https://github.com/Nifesimi-p/aws-networking/blob/main/NACL.png)
![NACL2](https://github.com/Nifesimi-p/aws-networking/blob/main/NACL2.png)
![SG](https://github.com/Nifesimi-p/aws-networking/blob/main/SG.png)
![IGW](https://github.com/Nifesimi-p/aws-networking/blob/main/IGW.png)
![RT](https://github.com/Nifesimi-p/aws-networking/blob/main/RT.png)





