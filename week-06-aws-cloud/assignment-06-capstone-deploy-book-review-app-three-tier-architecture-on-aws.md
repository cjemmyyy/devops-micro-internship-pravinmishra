# Assignment 6 — Capstone Assignment — Deploy Book Review App (Three-Tier Architecture) on AWS

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a fully production-style three-tier architecture on AWS: a Next.js Web Tier behind Nginx and a public ALB, a private Node.js/Express App Tier behind an internal ALB, and a private Multi-AZ MySQL RDS database with a read replica. You are expected to design, deploy, isolate, debug, and document the result independently.

---

# Task 1 — Architecture Diagram

## Goal

Create an architecture diagram showing the custom VPC (10.0.0.0/16), the six subnets across two Availability Zones (two public Web Tier, two private App Tier, two private Database Tier), the public ALB, Web Tier EC2/Nginx, internal ALB, private App Tier EC2, private Multi-AZ RDS with its read replica, and the permitted traffic flow.

### Evidence

#### Diagram image or link

![](screenshots/architecture2.png)

---

# Task 2 — AWS Region & Services Used

## Goal

Record the AWS Region used and list every AWS service used across networking, compute, load balancing, security, and the database.

### Notes

**Region:**

EU-NORTH-1 (Stockholm)

---

**Services:**

Custom VPC: Book-Review-VPC
6 subnets (private, public and database)
Internet Gateway
Elastic IP
NAT Gateway
3 route tables
3 security groups
Public ALB
Internal ALB
Target groups
Two EC2 instances (WEB and APP)
RDS MySQL (primary and read replica)
RDS subnet group

---

# Task 3 — Public Entry Point

## Goal

Confirm the Book Review App loads through the public ALB DNS name.

### Evidence

#### Public ALB DNS

Paste your public ALB DNS name here:

`Book-Review-Web-ALB-831147239.eu-north-1.elb.amazonaws.com`

---

# Task 4 — Evidence Screenshots

## Goal

Capture visual proof of every tier and load balancer.

### Evidence

#### Web EC2

![](screenshots/ASS6-SC1.png)

---

#### App EC2

![](screenshots/ASS6-SC2.png)

---

#### Public ALB

![](screenshots/ASS6-SC3.png)

---

#### Internal ALB

![](screenshots/ASS6-SC4.png)

---

#### RDS + Replica

![](screenshots/ASS6-SC5.png)

---

#### App UI proof

![](screenshots/ASS6-SC6.png)

---

# Task 5 — Summary

## Goal

Summarize what worked in the final deployment, the issues encountered and how each was fixed, and the tools or sources used to research and debug.

### Notes

**What worked:**

The App UI worked along with everything else.

---

**Issues + fixes:**

1. SSH connection timeout to private EC2 via bastion — port 22 timing out when hopping from the bastion to the private instance.
Root cause: private instance's security group allowed SSH from your IP instead of the bastion's security group.

2. MySQL connection timeout (Error 2003 / errno 110) — couldn't connect to the RDS endpoint on port 3306.
Same root cause pattern: RDS security group wasn't allowing inbound from the private EC2's security group.

3. Target group unhealthy / request timeout — ALB health checks failing.
Root cause: security group inbound rule was open on port 3301 instead of 3001, so the ALB couldn't reach the app at all.

4. "No books available" on the site — worked down the stack step by step:
Confirmed API (curl /api/books) was returning data fine on the server.
Confirmed the Books table had 3 rows.
Found the real issue in the browser: request was hitting /api/api/books — a duplicated /api prefix, caused by NEXT_PUBLIC_API_URL already being /api while page.js appended another /api/books on top of it.
Fixed in src/app/page.js.

---

**Tools/sources used:**

Google, Claude, AWS documentation, Medium articles.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post sharing the capstone deployment, including the public ALB DNS (or a redacted screenshot), three to five lines on what you built and why it is production-style, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/posts/joshua-chibuisi_aws-devops-devopsmicrointernship-share-7495275271344865280-_KIx/?utm_source=share&utm_medium=member_desktop&rcm=ACoAADNKSt0BkwUJXkGvXGi9tUas8IjHyH5UK9c`

---

#### Screenshot of LinkedIn post

![](screenshots/LINKEDIN1.png)

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, RDS credentials, connection strings, private keys, or account IDs

---

# Completion Checklist

- [x] Task 1: Architecture diagram completed
- [x] Task 2: AWS Region and services documented
- [x] Task 3: Public ALB DNS confirmed working
- [x] Task 4: All six evidence screenshots captured (Web Tier, App Tier, both ALBs, RDS + replica, app UI)
- [x] Task 5: Deployment summary completed (what worked, issues/fixes, tools/sources)
- [x] LinkedIn post published and URL submitted
- [x] App Tier and Database Tier confirmed not publicly accessible
- [x] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*