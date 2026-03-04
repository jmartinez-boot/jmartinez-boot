# 🏗️ Production-Grade AWS 3-Tier Web Application Architecture

This project demonstrates a **highly available, secure, and scalable** three-tier web application architecture deployed on AWS. It follows industry best practices for network isolation, security, and administrative access.

---

## 🧱 Architecture Overview

- **VPC** spanning 2 Availability Zones
- **Public, Private (App), and Private (Data)** subnets in each AZ
- **Internet Gateway** for public internet access
- **NAT Gateways** per AZ for outbound internet from private instances
- **Network ACLs** and **Security Groups** for defense-in-depth security
- **Bastion Host** for secure administrative access to private instances
- **Load Balancer** in public subnets to route traffic to app servers
- **Application Servers** in private app subnets
- **Database Servers** in private data subnets

---

## 🧩 Components & Configuration

### 1. VPC and Subnet Design

| Tier  | Subnet Type | Availability Zones | Purpose |
|-------|-------------|---------------------|---------|
| Web   | Public      | AZ1 & AZ2           | Load balancers, Bastion Host |
| App   | Private     | AZ1 & AZ2           | Application servers |
| Data  | Private     | AZ1 & AZ2           | Databases |

### 2. Routing & Internet Access

- **Internet Gateway** enables inbound/outbound internet for public subnets.
- **NAT Gateways** (one per AZ) allow private instances to access the internet for updates while remaining inaccessible from the internet.

### 3. Security

#### Network ACLs (Subnet-level firewall)

| Tier   | Inbound Rules                          | Outbound Rules                     |
|--------|----------------------------------------|-------------------------------------|
| Public | Allow 80/443/22 from 0.0.0.0/0         | Allow ephemeral ports return traffic |
| App    | Allow from Public subnet (80/443)      | Allow to Public & Data subnets      |
| Data   | Allow from App subnet (DB port e.g., 3306) | Allow return to App subnet       |

#### Security Groups (Instance-level firewall)

| Security Group     | Inbound Rules                          | Source                          |
|--------------------|----------------------------------------|----------------------------------|
| Public ELB SG      | HTTP (80), HTTPS (443)                 | 0.0.0.0/0                       |
| Bastion Host SG    | SSH (22)                                | Trusted admin IP range          |
| App Server SG      | HTTP (80), HTTPS (443)                  | ELB Security Group              |
| DB Server SG       | MySQL (3306) / Custom DB port           | App Server Security Group       |

### 4. Secure Administrative Access

- Bastion Host in public subnet allows SSH only from authorized IPs.
- From Bastion, admins connect to private app servers via internal network.

---

## 🖼️ Architecture Diagrams

*Include screenshots from the PDF here:*

- VPC and subnet layout
- Route tables and NAT gateways
- Network ACL rules
- Security group rules
- Bastion host connection flow

---

## 🧪 Results & Validation

- Successfully SSH’d into Bastion Host from local machine.
- From Bastion, connected to private App Server via internal IP.

```bash
# From local machine
ssh -A ec2-user@<bastion-public-ip>

# From Bastion to App Server
ssh ec2-user@<app-server-private-ip>
<!--
**jmartinez-boot/jmartinez-boot** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
