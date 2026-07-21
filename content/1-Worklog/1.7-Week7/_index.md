---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Learn to gather infrastructure operational metrics using Amazon CloudWatch.
* Establish automated error alerts to notify administrators via email.
* Build Dashboards to visualize the health status of cloud resources.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Study Amazon CloudWatch: metrics, namespaces, dimensions, and resolutions. | 05/25/2026 | 05/25/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | Configure CloudWatch Alarms to monitor EC2 CPU utilization (>80%) and system status checks. | 05/26/2026 | 05/26/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 - 4 | Create an Amazon SNS topic, subscribe an email endpoint, confirm the subscription from the inbox, and link the topic to a CloudWatch Alarm. | 05/26/2026 | 05/27/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Study CloudWatch Logs. Install and configure the CloudWatch Agent on EC2 to push system and application logs. | 05/28/2026 | 05/28/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Practice: View the `EstimatedCharges` metric in `us-east-1`, create a Billing Alarm, and build a CloudWatch Dashboard for EC2 and ALB metrics. | 05/29/2026 | 05/29/2026 | https://cloudjourney.awsstudygroup.com/ |

### Week 7 Achievements:

* Created alarms for EC2 CPU and status checks; inspected the `OK`, `INSUFFICIENT_DATA`, and `ALARM` states against metric data.
* Confirmed the SNS email subscription before testing alarm notifications.
* Installed the CloudWatch Agent on EC2 and verified the log stream receiving system and application logs.
* Created a Billing Alarm from `EstimatedCharges` in `us-east-1`. The alarm sends notifications and does not stop resources automatically.
