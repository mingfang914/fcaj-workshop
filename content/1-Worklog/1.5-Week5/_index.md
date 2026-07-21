---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Study relational (RDS) and NoSQL (DynamoDB) database engines on AWS.
* Deploy highly available relational databases isolated inside private subnets.
* Learn NoSQL data operations and index configurations in DynamoDB.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Study Amazon RDS: Database engine types, Multi-AZ deployments for high availability, and Read Replicas. | 05/11/2026 | 05/11/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | Launch an Amazon RDS MySQL instance in private subnets. Configure DB Subnet Groups and Security Groups to restrict access. | 05/11/2026 | 05/12/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Connect from EC2 to RDS MySQL through the DB endpoint and run test queries. On the Multi-AZ configuration, run `Reboot with failover` and observe the connection interruption. | 05/13/2026 | 05/14/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Study Amazon DynamoDB NoSQL: Partition Keys (PK), Sort Keys (SK), Global Secondary Indexes (GSI), and Local Secondary Indexes (LSI). | 05/14/2026 | 05/14/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Practice: Create a DynamoDB table on the console and perform basic CRUD operations using PartiQL and AWS CLI. | 05/15/2026 | 05/15/2026 | https://cloudjourney.awsstudygroup.com/ |

### Week 5 Achievements:

* Launched RDS MySQL in private subnets and restricted its Security Group to the EC2 instance used by the lab.
* Ran `Reboot with failover` on the Multi-AZ configuration and checked reconnection through the same DB endpoint.
* Compared Partition Key, Sort Key, GSI, and LSI behavior using sample access patterns.
* Ran DynamoDB CRUD operations through PartiQL and AWS CLI, then removed the lab resources.
