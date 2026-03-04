## PRODUCTION-GRADE

NETWORK ARCHITECTURE IN

AWS HOSTING A STANDARD

THREE-TIER WEB

APPLICATION.

CLOUD ENGINEERING BELIZE

GDS  👋

**Network Configuration and Implementation**

_1. VPC and Subnet Structure_

A single VPC is created to house all resources. It is configured to span two Availability Zones for
high availability.

Tier               Subnet Type            Availability Zones          Purpose

Public             Public                 AZ 1 & AZ 2                 Internet-facing Load Balancers and Bastion Hosts.

App                 Private               AZ 1 & AZ 2                 Application Servers; outbound internet access via                                                                       NAT Gateway.

Data                Private               AZ 1 & AZ 2                 Databases; no direct internet access (inbound or                                                                        outbound).

_2. Connectivity and Routing_

• Internet Gateway (IGW): Attached to the VPC to allow communication between
resources in the public subnets and the internet.

• NAT Gateways: One NAT Gateway is deployed into each public subnet. Private subnets
are configured to route all outbound internet-bound traffic (0.0.0.0/0) through their
respective AZ's NAT Gateway. This provides outbound internet access for tasks like
software updates without exposing the instances to inbound requests.

_3. Security Implementation_

Security is enforced using a defense-in-depth approach with both Security Groups (stateful) and
Network ACLs (stateless).

_Network ACLs (NACLs)_

NACLs act as firewalls at the subnet level. The rules below represent the core security posture.

Tier              Inbound Rules                                                 Outbound Rules

Public            Allow all traffic from internet (0.0.0.0/0) to                Allow all return traffic (ephemeral ports).
                  allowed ports (80/443/22).

App (Private)    Allow traffic from Public Subnet SG (port 443/80).             Allow return traffic to Public SG, and to Data                                                                               SG (DB port).

Data (Private)   Allow traffic only from App Subnet SG (DB port e.g., 3306 ).   Allow return traffic to App SG.

_Security Groups (SGs)_

Security Groups act as firewalls at the instance level, allowing granular control based on specific
resource roles.

Security Group                Inbound Rules (Source)                    Description
  
Public ELB SG                 Port 80, 443                              Fronts the application, accepts traffic from users.  
                              (Internet 0.0.0.0/0)

Bastion Host SG               Port 22 (Admin IP range)                  Allows SSH access only from a trusted IP range for
                                                                        administration.

App Server SG                 Port 80/443 (ELB SG)                      Accepts traffic only from the load balancer SG.

DB Server SG                  DB Port (App Server SG)                   Accepts traffic only from the application server SG.

_4. Secure Administrative Access_

A "Bastion Host" (or jump box) EC2 instance is placed in a public subnet. Administrators connect
via SSH to the Bastion host using strong credentials (SSH keys), and then from the bastion,
connect internally to application servers in the private subnets. Access to the bastion is tightly
restricted via its security group to specific source IP addresses.
This architecture provides a robust, secure, and scalable foundation for the three-tier web
application.

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
