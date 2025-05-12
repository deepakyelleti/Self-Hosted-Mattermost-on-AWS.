# Self-Hosted Mattermost on AWS

## Overview
Deploy a self-hosted instant-messaging platform (Mattermost) on AWS. This solution uses:
- A *public subnet* for the application server  
- A *private subnet* for a MySQL database  
- Standard AWS networking components (VPC, Internet Gateway, Route Tables, NAT Gateway)  

This architecture ensures complete control over infrastructure and data, in line with organizational policies. :contentReference[oaicite:0]{index=0}:contentReference[oaicite:1]{index=1}

---

## Architecture Diagram
![AWS VPC Architecture](architecture.png)

---

## Demo Video
A step-by-step walkthrough of the full deployment process:

▶ [Watch the video demo](video/demo.mp4)

*Place your demo.mp4 file inside the video/ directory before publishing.*

---

## Prerequisites
- AWS account with permissions to create VPCs and EC2 instances  
- AWS CLI or Console access  
- Basic familiarity with AWS networking (VPC, subnets, security groups)

---

## Deployment Steps

1. *Create a VPC*  
   - Define CIDR block (e.g. 10.0.0.0/16)  
   - Enable DNS hostnames

2. *Create a Public Subnet*  
   - Place application server here  
   - Associate with Internet Gateway

3. *Create a Private Subnet*  
   - Place MySQL database server here  
   - No direct Internet access

4. *Create and Attach an Internet Gateway*  
   - Attach to your VPC  
   - Allows public subnet to reach the Internet

5. *Configure Public Route Table*  
   - Route 0.0.0.0/0 → Internet Gateway  
   - Associate with public subnet

6. *Deploy a NAT Gateway*  
   - In the public subnet  
   - Allows private subnet instances outbound Internet access

7. *Configure Private Route Table*  
   - Route 0.0.0.0/0 → NAT Gateway  
   - Associate with private subnet

8. *Launch the Application Server (EC2)*  
   - In the public subnet  
   - Install prerequisites (Docker, etc.)

9. *Launch the Database Server (EC2)*  
   - In the private subnet  
   - Harden security group to allow only application server

10. *Install & Configure MySQL*  
    - Secure installation (mysql_secure_installation)  
    - Create Mattermost database and user

11. *Install & Configure Mattermost*  
    - Download Mattermost server  
    - Configure config.json to point at your MySQL endpoint  
    - Start Mattermost service

---

## Verification & Usage
- *Access Mattermost* via the application server’s public IP or DNS  
- *Log in* with the default sysadmin account, then create new teams, channels, and users  

---

## Result
You should see a fully operational Mattermost instance, with data persisted in your private-subnet MySQL database.  

---

## Author
*Deepak Yelleti* (Student ID: A0000074884)  
COMP-692-AH1 – Topics in Computer Science, Rivier University  
Professor: Dr. Renu Paldiwal  
Date: February 25, 2025 :contentReference[oaicite:2]{index=2}:contentReference[oaicite:3]{index=3}

---
