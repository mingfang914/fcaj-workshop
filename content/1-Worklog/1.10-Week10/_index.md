---
title: "Week 10 Worklog"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Configure secure user sign-ups and logins using Amazon Cognito.
* Deploy Role-Based Access Control (RBAC) using User Groups.
* Write custom least-privilege IAM policies and execution roles for backend Lambdas.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Create Amazon Cognito User Pool with an SPA Web Client (no secret) using the quick-setup wizard. | 06/15/2026 | 06/15/2026 |  |
| 3 | Configure custom attribute `custom:role` and create user groups (`admin`, `user`) in Cognito. | 06/16/2026 | 06/16/2026 |  |
| 4 | Analyze Lambda backend permission requirements for S3, DynamoDB, and Cognito, designing JSON permission blocks. | 06/17/2026 | 06/17/2026 |  |
| 5 | Create the custom IAM Policies (`SmartImage-ApiHandler-Policy`, `SmartImage-ImageProcessor-Policy`, `SmartImage-AIAnalyzer-Policy`) under Access Management. | 06/18/2026 | 06/18/2026 |  |
| 6 | Practice: Create Lambda execution roles and attach the respective custom policies to finalize authorization setups. | 06/19/2026 | 06/19/2026 |  |

### Week 10 Achievements:

* Created the Amazon Cognito User Pool and configured the SPA Web Client with client secrets disabled, facilitating direct client-side authentication from React.
* Added the custom attribute `custom:role` and established Cognito groups (`admin` with precedence 0, `user` with precedence 10) to support Role-Based Access Control (RBAC).
* Designed and created custom IAM policies under Access Management (`SmartImage-ApiHandler-Policy`, `SmartImage-ImageProcessor-Policy`, `SmartImage-AIAnalyzer-Policy`) to ensure clean execution role associations.
* Configured three Lambda execution roles with least-privilege permissions, granting secure access to specific S3 buckets, DynamoDB tables, and Amazon Rekognition APIs.
