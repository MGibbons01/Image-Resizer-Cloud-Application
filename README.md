# ☁️ Image-Resizer-Cloud-Application

## 📸 Project Overview

This project involved developing a highly available and secure photo album website on AWS. It includes automated scalability, Lambda-powered image processing, database integration, and security-hardened components.

While the live deployment is **no longer active**, this repository contains a **comprehensive PDF report** with **screenshots** and **explanations** documenting the setup, configuration, and testing process of all components.



## 📊 Architectural Diagram


![Architectural Diagram](https://github.com/MGibbons01/Image-Resizer-Cloud-Application/blob/main/ArchitecturalDiagram.png?raw=true)



## 🏗️ Infrastructure Components

The infrastructure was designed for high availability, security, and scalability, and includes the following:

### 🔹 VPC & Subnets
- **Custom VPC** with **two Availability Zones** (`Zone A` and `Zone B`)
- **Public and Private Subnets** in each zone for web and backend resources

### 🔹 Internet Gateway & Route Tables
- Internet Gateway attached to VPC
- Route tables configured to allow public internet access for necessary components

### 🔹 NAT Gateway
- NAT Gateway deployed in a **public subnet** to allow **private instances** outbound internet access without exposing them

### 🔹 EC2 Web Servers
- A **Dev instance** was configured with Apache, PHP, MySQL, and AWS SDK
- An **AMI** was created from this instance and used to configure a **Launch Template**
- **Auto Scaling Group** deployed **2 private web instances** using the Launch Template
- **Elastic Load Balancer (ELB)** distributes traffic to web instances and performs health checks on `/photoalbum/album.php`

### 🔹 S3 Bucket
- S3 Bucket from Assignment 1b reused
- Bucket policy updated to **only allow access from web server IP**
- Public HTTP access was **explicitly denied**

### 🔹 Lambda Function
- A **Lambda Function** was created to automatically generate **image thumbnails**
- Function triggered by S3 uploads, with permissions managed through **IAM roles**

### 🔹 RDS (MySQL)
- **RDS instance** created for storing photo metadata
- Database schema configured via **phpMyAdmin** on web server

### 🔹 Security Groups & NACLs
- **Security Groups** defined for:
  - Dev Server
  - Web Servers
  - Load Balancer
  - RDS Database
- **Network ACLs (NACLs)** used to block ICMP traffic to/from Dev Server for added security

### 🔹 IAM Roles
- IAM Role assigned to Lambda to allow interaction with S3 and CloudWatch securely


## 🧪 Functionality & Testing

The application was tested by uploading `.png` images and metadata via `photouploader.php`. The process:
1. Uploaded photo stored in S3
2. Lambda resized image to thumbnail
3. Metadata stored in RDS
4. `album.php` displayed both thumbnails and data in a table


