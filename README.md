# Self-Hosted Mattermost on AWS

## Overview
Deploy a self-hosted instant-messaging platform (Mattermost) on AWS. This solution uses:
- A *public subnet* for the application server  
- A *private subnet* for a MySQL database  
- Standard AWS networking components (VPC, Internet Gateway, Route Tables, NAT Gateway)  

This architecture ensures complete control over infrastructure and data, in line with organizational policies.

---

## Architecture Diagram
![Screenshot 2025-02-23 190906](https://github.com/user-attachments/assets/782a64e5-aec7-4d09-b294-f0dc93cb5672)

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
![Screenshot 2025-02-24 102733](https://github.com/user-attachments/assets/ed639f75-34e9-49d4-accf-474db6e38fce)
![Screenshot 2025-02-24 103137](https://github.com/user-attachments/assets/707f73d8-ab3b-41ef-b0bf-5e3418b6d210)

2. *Create a Public Subnet*  
   - Place application server here  
   - Associate with Internet Gateway
![Screenshot 2025-02-24 105223](https://github.com/user-attachments/assets/7b307f4f-a55a-4df9-9bb8-9163bd1a644a)
![Screenshot 2025-02-24 105241](https://github.com/user-attachments/assets/0ae453c8-e5b6-4571-99ff-44303b56b383)
![Screenshot 2025-02-24 105604](https://github.com/user-attachments/assets/d47a1c8d-4ea6-4e24-9dcc-976ba99210b9)
![Screenshot 2025-02-24 105727](https://github.com/user-attachments/assets/847d42fe-7ecf-4616-90ff-2c62ae18cda4)
![Screenshot 2025-02-24 105810](https://github.com/user-attachments/assets/2febc7f3-96ee-4c51-9f59-0ba48a366d03)

3. *Create a Private Subnet*  
   - Place MySQL database server here  
   - No direct Internet access
![Screenshot 2025-02-24 110238](https://github.com/user-attachments/assets/85846d1f-75c6-4025-acdc-f2773fac427b)
![Screenshot 2025-02-24 110306](https://github.com/user-attachments/assets/33c6735a-ac93-44c2-9e7a-089f333a7397)
![Screenshot 2025-02-24 110415](https://github.com/user-attachments/assets/ebd50f13-25ba-4138-a0d5-6e616bf90eb5)
![Screenshot 2025-02-24 110959](https://github.com/user-attachments/assets/48911f5c-15d8-41bc-b687-e86e022b3628)

4. *Create and Attach an Internet Gateway*  
   - Attach to your VPC  
   - Allows public subnet to reach the Internet
![Screenshot 2025-02-24 112313](https://github.com/user-attachments/assets/b10ffabb-cb87-4fd9-ab17-7ba244eaabaa)
![Screenshot 2025-02-24 112634](https://github.com/user-attachments/assets/4dc7773b-2398-4332-98d6-fc0deebbdfb1)
![Screenshot 2025-02-24 113150](https://github.com/user-attachments/assets/0cba5c47-86d4-44d6-966a-db709f8e9589)
![Screenshot 2025-02-24 113310](https://github.com/user-attachments/assets/10953c15-b528-41ec-9f5b-1697ff385f3d)
![Screenshot 2025-02-24 113616](https://github.com/user-attachments/assets/5a879f39-7065-468e-af5d-540bfc897180)

5. *Configure Public Route Table*  
   - Route 0.0.0.0/0 → Internet Gateway  
   - Associate with public subnet
![Screenshot 2025-02-24 114239](https://github.com/user-attachments/assets/96a5813b-a11c-4905-8739-f3ad6a889251)
![Screenshot 2025-02-24 114306](https://github.com/user-attachments/assets/7c6632d2-4318-4114-b551-fc2d44f246ed)
![Screenshot 2025-02-24 114556](https://github.com/user-attachments/assets/c5c6e121-509e-4dd2-bb07-ee0b8b09e87b)
![Screenshot 2025-02-24 114803](https://github.com/user-attachments/assets/ae561b6a-6d28-4126-80c8-8c64c92e3716)
![Screenshot 2025-02-24 124745](https://github.com/user-attachments/assets/c35b190a-5cc8-4376-85cf-2af1df613e41)
![Screenshot 2025-02-24 114949](https://github.com/user-attachments/assets/80abf638-397a-4f33-b535-6340e0276eff)
![Screenshot 2025-02-24 115516](https://github.com/user-attachments/assets/ca4956b6-0825-48d0-84ef-1374bec380bc)
![Screenshot 2025-02-24 115629](https://github.com/user-attachments/assets/0e2a8427-378d-4034-81fc-3858db622b8d)
![Screenshot 2025-02-24 120002](https://github.com/user-attachments/assets/609f5174-80f4-4a0d-8d8e-94391eda8a4a)

6. *Deploy a NAT Gateway*  
   - In the public subnet  
   - Allows private subnet instances outbound Internet access
![Screenshot 2025-02-24 120442](https://github.com/user-attachments/assets/91b42793-cd39-4a00-a9e3-b1cdf6b4a9c5)
![Screenshot 2025-02-24 120816](https://github.com/user-attachments/assets/bbb34985-2370-488e-bd31-612bdec8f66e)
![Screenshot 2025-02-24 121021](https://github.com/user-attachments/assets/3dac038a-6dd7-470e-bcdc-19a606b9ca43)
![Screenshot 2025-02-24 121243](https://github.com/user-attachments/assets/a846c2c5-8ae2-495e-841e-5f868a93cb18)

7. *Configure Private Route Table*  
   - Route 0.0.0.0/0 → NAT Gateway  
   - Associate with private subnet
![Screenshot 2025-02-24 121908](https://github.com/user-attachments/assets/30da0888-e772-455a-87dc-86b9aaf3be76)
![Screenshot 2025-02-24 122030](https://github.com/user-attachments/assets/5e7b53fe-27e7-410e-84e6-5875b382eb62)
![Screenshot 2025-02-24 122634](https://github.com/user-attachments/assets/a4580e73-6a4c-4bf7-afb8-f57814cec8bb)
![Screenshot 2025-02-24 123525](https://github.com/user-attachments/assets/5b59c068-201f-4f10-a03a-1a59dc108978)
![Screenshot 2025-02-24 124029](https://github.com/user-attachments/assets/2d42fd66-ec7a-47c0-83fb-9aa442e7f015)
![Screenshot 2025-02-24 124316](https://github.com/user-attachments/assets/609b878a-7492-4d39-8384-13044fb4e4e3)
![Screenshot 2025-02-24 124435](https://github.com/user-attachments/assets/dbce2b40-6255-4d0b-8374-5bf42667da4e)
![Screenshot 2025-02-24 124452](https://github.com/user-attachments/assets/176c0d01-dac2-40fb-afee-da892c5d1262)
![Screenshot 2025-02-24 124637](https://github.com/user-attachments/assets/acbfb0be-092b-42f0-b059-06c0c8fec46c)

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
Date: February 25, 2025

---
