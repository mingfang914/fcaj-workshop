---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

* Study traffic distribution and automatic resource scaling on AWS.
* Deploy an Application Load Balancer to route client requests.
* Configure an Auto Scaling Group to dynamically scale instance count based on load.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Study Elastic Load Balancing (ELB) types: ALB, NLB, GLB. Configure Target Groups and HTTP health checks. | 05/18/2026 | 05/18/2026 | https://cloudjourney.awsstudygroup.com/ |
| 2 - 3 | Deploy an Application Load Balancer (ALB) in Public Subnets. Route HTTP requests to web servers running in Private Subnets. | 05/18/2026 | 05/19/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Study Auto Scaling components: Launch Templates, Minimum/Maximum/Desired capacity settings, and cooldown periods. | 05/20/2026 | 05/20/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Configure an Auto Scaling Group (ASG) behind the ALB. Set up a Target Tracking Scaling Policy targeting Average CPU Utilization (e.g., 50%). | 05/21/2026 | 05/21/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 - 6 | Practice: Install Apache on EC2 instances. Run an Apache Bench (`ab`) load test to artificially spike CPU, validating automated scale-out and scale-in. | 05/21/2026 | 05/22/2026 | https://cloudjourney.awsstudygroup.com/ |

### Week 6 Achievements:

* Configured Target Groups, HTTP health checks, and launched an Application Load Balancer (ALB) in Public Subnets.
* Created Launch Templates and deployed an Auto Scaling Group (ASG) behind the ALB across multiple AZs.
* Set up a Target Tracking Scaling Policy to dynamically scale instance count targeting average CPU utilization.
* Performed stress tests using Apache Bench (ab), verifying automated scale-out and scale-in metrics under CPU load.
