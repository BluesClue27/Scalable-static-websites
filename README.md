# Scalable Website Hosting on EC2

This project hosts a static website (HTML,CSS,JS) on an **Auto Scaling Group (ASG)** of **EC2 instances** behind an **Application Load Balancer (ALB)** in AWS. The website is automatically deployed using **Launch Templates** and **User Data Scripts**.

This project focuses on designing a simple Virtual Private Cloud (VPC) architecture using multiple AWS services, while also integrating technologies such as Apache Web Servers, Bash scripts, and front-end development with HTML, CSS, and JavaScript. The goal is to implement a secure, scalable, and cost-effective cloud-based solution for static website hosting. 

## Technologies Used
- Virtual Private Cloud (VPC)  
Provides an isolated network environment within AWS to ensure secure and controlled communication between AWS resources, acting as the foundation for cloud infrastructure
- Subnets:
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
Below is a high-level architecture diagram of the setup

![Architecture Diagram](/images/V1_StaticWebsiteHosting.png)

### 1. Client Request (User Accessing Website)
The user (client) accesses the website using a browser and enters the DNS name of the application. 
This request is sent over the internet to the AWS cloud infrastrucure. 

### 2. Internet Gateway (IGW)
The request first reaches the **Internet Gateway (IGW)**, which allows communication between public internet users and AWS resources inside the **VPC**.
The IGW ensures that incoming and outgoing traffic can be routed to instances in public subnets

### 3. Application Load Balancer (ALB)
The request is then forwarded to an **Application Load Balancer**, which distributes traffic across multiple **EC2 instances** within the **Auto Scaling Group (ASG)**.
This improves availability and fault tolerance by ensuring requests are handled by healthy instances across different **Availability Zones (AZs)**.

Each **EC2 instance** in the **ASG** is hosted in a **Public Subnet**, allowing it to serve static website files (HTML, CSS, JS).

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
- Enable public IP allocation for subnets so that EC2 instances receive a public IP and can be accessed from the internet.

### 5. Create a Security Group
- EC2 Security Group: Allow HTTP(80) from anywhere and SSH(22) only from trusted IPs 
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
- Attach to an existing load balancer and configure health checks to ensure instances are replaced if they fail
- Set minimum and maximum desired capacity for your ASG

## Why Build This?

This project was created as a learning exercise to:

1. Explore **AWS services** like VPC, Subnet, Internet Gateway, Route Tables, Application Load Balancers, Auto Scaling Groups, EC2, Security Groups.
2. Explore other software like Apache Web Servers, Bash Scripts, HTML, CSS, Javascript. 
3. Understand how to implement secure, scalable, and cost-effective solutions for website hosting on the cloud.

## Deployment Verification
- Visit the Load Balancer's DNS name in a browser to confirm the websites are accessible
- SSH into an instance and check if Apache is running 
```bash
sudo systemctl status httpd
```
