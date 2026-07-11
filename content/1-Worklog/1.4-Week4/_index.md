---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Learn the core components of Amazon VPC virtual networking.
* Design secure, partitioned network infrastructures across multiple Availability Zones (AZs).

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Study VPC concepts: IP addressing, CIDR blocks, and subnetting. Design CIDR scopes for a custom VPC. | 05/04/2026 | 05/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 - 4 | Create Public and Private Subnets across two Availability Zones. Set up Route Tables and associate them, and attach an Internet Gateway (IGW). | 05/05/2026 | 05/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Provision a NAT Gateway in a Public Subnet. Route outbound internet traffic from Private Subnets through the NAT Gateway. | 05/06/2026 | 05/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Compare stateful Security Groups with stateless Network ACLs (NACLs). Configure firewall rules for both network layers. | 05/07/2026 | 05/07/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Practice: Deploy a Bastion Host in a Public Subnet and a web server in a Private Subnet. SSH into the private instance via the Bastion Host and verify NAT internet access. | 05/08/2026 | 05/08/2026 | https://cloudjourney.awsstudygroup.com/ |

### Week 4 Achievements:

* Designed a custom Amazon VPC network, partitioning public and private subnets across multiple Availability Zones (AZs).
* Configured Route Tables, Internet Gateways for public subnets, and NAT Gateways for private subnet outbound routing.
* Set up two-tier firewall configurations combining stateful Security Groups and stateless Network ACLs (NACLs).
* Deployed a Bastion Host jump box in a public subnet for secure SSH administration into private subnet web servers.
