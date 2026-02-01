# aws-ec2-vpc-down-server-dilemma

Designing Reliable EC2 Access with Proper VPC Networking

---

## The Down Server Dilemma — DataForge Analytics (EC2 + VPC)

This project documents how I diagnosed and resolved repeated EC2 access failures
caused by public IP changes, subnet routing issues, and restrictive network
configurations inside an AWS VPC.

Rather than simply restoring access, the focus was on designing the environment
for **reliability, security, and repeatability**.

---

## 🚀 Project Overview

A DataForge Analytics EC2 instance became unreachable after restarts due to
misaligned subnet routing, public IP behavior, and SSH access restrictions.

This project demonstrates how to properly design a **public subnet architecture**
so compute resources remain accessible, observable, and secure across restarts
and handoffs.

---

## 🛠️ Technologies Used

- AWS EC2
- Amazon VPC
- Public & Private Subnets
- Route Tables
- Internet Gateway
- Security Groups
- SSH (Key Pair Authentication)
- Amazon CloudWatch
- Amazon SNS

---

## ⚙️ Investigation & Design Steps

1. Reviewed EC2 instance state and health checks
2. Attempted browser and SSH access using public IPv4 and DNS
3. Identified intermittent access failures after instance restarts
4. Audited subnet configuration and route table associations
5. Verified Internet Gateway attachment to the VPC
6. Confirmed `0.0.0.0/0` routing from public subnets to the IGW
7. Enabled auto-assign public IPv4 at the subnet level
8. Restricted SSH (port 22) access to **My IP** only
9. Validated key pair authentication using SSH
10. Added observability and alerting for system health

---

## 🧩 Root Cause

- EC2 instance launched into a subnet not fully configured for public access
- Public IPv4 auto-assignment disabled at the subnet level
- Route table associations inconsistent across subnets
- SSH access blocked due to IP/region mismatch
- No monitoring in place to detect system stress or risk

---

## 🛠️ Solution

- Designed a proper **public subnet** with:
  - Internet Gateway attached to the VPC
  - Route table with `0.0.0.0/0 → IGW`
  - Explicit subnet-to-route-table association
  - Public IPv4 auto-assignment enabled
- Secured administrative access:
  - SSH allowed only from **My IP**
  - Key pair authentication enforced
- Added observability:
  - CloudWatch CPUUtilization alarm (>80% for 5 minutes)
  - SNS email notifications for proactive alerting

---

## 🧪 Verification

- Successfully connected to the instance via SSH using a `.pem` key
- Verified public access using IPv4 and public DNS
- Confirmed outbound internet access from the EC2 instance
- Observed CloudWatch metrics and alarm state transitions
- Received SNS alert notifications as expected

---

## ✅ Result

- Stable and repeatable EC2 access restored
- Secure network boundaries enforced
- Monitoring and alerting enabled
- Infrastructure documented for future engineers
- Cost: **$0 (AWS Free Tier)**

---

## 📚 Key Learnings

- How subnet route tables directly impact EC2 accessibility
- Why public IP behavior matters for uptime
- How overly permissive SSH rules increase attack surface
- Why observability is essential for production systems
- How to design infrastructure that survives restarts

---

## 💬 Reflection

A consultant doesn’t just launch infrastructure — they design for uptime,
scale, and repeatability.

**Reflection Questions:**
1. Why does assigning a public IP make an EC2 instance reachable by clients?
2. What could happen if SSH (port 22) is opened to “Anywhere” instead of “My IP”?
3. How would you explain to a business owner why adding a Load Balancer or Elastic
   IP improves reliability?

---

## 🔜 Next Steps

- Introduce Elastic IP for persistent addressing
- Add Application Load Balancer
- Implement Auto Scaling
- Convert infrastructure to Terraform
- Expand monitoring to disk and memory metrics



