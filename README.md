# aws-ec2-vpc-down-server-dilemma
Designing Reliable EC2 Access with Proper VPC Networking
The Down Server Dilemma — DataForge Analytics (EC2 + VPC)

This project documents how I diagnosed and resolved repeated EC2 access failures
caused by public IP changes, subnet routing issues, and restrictive network
configurations. The goal was not just to restore access, but to design the
infrastructure so future engineers would not need to re-debug the same problem.

🚀 Project Overview

DataForge Analytics experienced intermittent downtime when an EC2-hosted analytics
dashboard became unreachable after instance restarts. Each restart introduced a
new public IP, breaking client access and administrative connectivity.

This project focuses on network design, security boundaries, and observability
rather than simply launching compute.

🛠️ Technologies Used

AWS EC2

Amazon VPC

Public & Private Subnets

Route Tables

Internet Gateway

Security Groups

SSH (Key Pair Authentication)

AWS CloudWatch

Amazon SNS

⚙️ Investigation & Design Process

Verified EC2 instance health and state

Attempted browser and SSH access using public IPv4 and DNS

Reviewed subnet configuration and route table associations

Identified missing or incorrect routes to the Internet Gateway

Validated public IPv4 auto-assignment at the subnet level

Audited Security Group inbound rules for SSH scope

Confirmed region and IP alignment for administrative access

Reviewed lack of monitoring and alerting for system stress

🧩 Root Cause

EC2 instance was deployed into a subnet that was not fully configured for
public access

Public IPv4 assignment was disabled at the subnet level

Route table associations were inconsistent

SSH access was restricted due to IP/region mismatch

No observability existed to detect or alert on performance risk

🛠️ Solution Implementation

Designed a proper public subnet architecture:

Internet Gateway attached to the VPC

Route table with 0.0.0.0/0 → IGW

Explicit subnet-to-route-table association

Enabled auto-assign public IPv4

Restricted SSH access using least-privilege Security Group rules

SSH allowed only from My IP

Validated secure access using an EC2 key pair

Added CloudWatch observability:

CPUUtilization alarm (>80% for 5 minutes)

SNS email notifications for proactive alerting

🧪 Validation

Successfully connected to the instance via SSH using a private key

Verified public accessibility through IPv4 and public DNS

Confirmed outbound traffic routing through Internet Gateway

Observed CloudWatch metrics and alarm state changes

Received SNS alert notifications as expected

✅ Result

Reliable and repeatable public access restored

Secure administrative access enforced

Proactive monitoring enabled

Infrastructure behavior documented for future engineers
