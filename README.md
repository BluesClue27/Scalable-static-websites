# Scalable Website Hosting on EC2

This project hosts a static website (HTML,CSS,JS) on an **Auto Scaling Group (ASG)** of **EC2 instances** behind an **Application Load Balancer (ALB)** in AWS. The website is automatically deployed using **Launch Templates** and **User Data Scripts**.

## Technologies Used
- Virtual Private Cloud (VPC)
- Subnets
- Internet Gateway
- Route Table
- Application Load Balancer (ALB)
- Auto Scaling Group (ASG)
- Security Groups
- Elastic Compute Cloud (EC2)
- Launch Templates and User Data Scripts

## Software
- Apache (Web Server)
- Bash (User Data Script)
- HTML, CSS, Javascript

## Architecture Diagram

<p align="center">
   <img src="/images/V1_StaticWebsiteHosting.png" height=400 >
<p>

### 1. Client Request (User Accessing Website)
The user (client) accesses the website using a browser and enters the DNS name of the application. 
This request is sent over the internet to the AWS cloud infrastrucure. 

### 2. Internet Gateway (IGW)
The request first reaches the **Internet Gateway (IGW)**, which allows communication between public internet users and AWS resources inside the **VPC**.
The IGW ensures that incoming and outgoing traffic can be routed to instances in public subnets

### 3. Application Load Balancer (ALB)
The request is then forwarded to an **Application Load Balancer**, which distributes traffic across multiple **EC2 instances** within the **Auto Scaling Group (ASG)**.
This improves availability and fault tolerenace by ensuring requests are handled by healthy instances across different **Avilability Zones (AZs)**.

Each **EC2 instance** the **ASG** is hosted in a **Pulic Subnet** and can serve static website files (HTML, CSS, JS).

## Setup & Deployment Steps
### 1. Create VPC & Networking
- Create a VPC and establish IP Address Range Pool (i.e. boundaries)
- Be sure to click 'Default' Tenancy as this is the most cost-efficient option as you are sharing the costs of the underlying hardware with other AWS customers. 
- Enable DNS Hostname Resolution for VPC

### 2. Subdivide IP Range into Subnets
- Further divide IP Address Range into smaller segments called subnets
- This ensures further isolation of AWS resources

### 3. Create Route Tables
- Route all outgoing traffic to the Internet Gateway

### 4. Create Internet Gateway
- The Internet Gateway serves as the gateway that connects your VPC to the wider internet; it acts as an access point
- Enable public IP Configuration for Subnet to allow your instances to have public IP addresses

### 5. Create a Security Group
- EC2 Security Group: Allow HTTP(80) and SSH(22)
- ALB Security Group: Allow HTTP(80) from the Internet

### 6. Create an Application Load Balancer
- Ensure that it is an Internet-facing Load Balancer
- Include your newly created VPC and the AZs
- Include the ALB security group created

### 7. Create a launch template for your Auto Scaling Groups
- Configure the EC2 instances - AMI; instance type; key pair; security groups; user data script

### 8. Create an Auto Scaling Group
- Include the launch template created
- Include the newly created VPC and AZs
- Attach to an existing load balancers
- Set minimum and maximum desired capacity for your ASG

## Why Build This?

This project was created as a learning exercise to:

1. Explore **AWS services** like VPC, Subnet, Internet Gateway, Route Tables, Application Load Balancers, Auto Scaling Groups, EC2, Security Groups.
2. Expore tther software like Apache Web Servers, Bash Scripts, HTML, CSS, Javascript. 
3. Understand how to implement secure, scalable, and cost-effective solutions for website hosting on the cloud.
---
