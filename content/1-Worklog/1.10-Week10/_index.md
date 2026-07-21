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
* Declare backend execution roles and least-privilege permissions with AWS CDK.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Declare a Cognito User Pool and an SPA app client without a client secret in CDK; inspect the deployed resources in the Cognito Console. | 06/15/2026 | 06/15/2026 | `infrastructure/lib/stacks/auth-stack.ts` |
| 2 - 3 | Create the `admin` and `user` Cognito groups; define the `cognito:groups` claim as the authorization source used by the frontend and backend. | 06/15/2026 | 06/16/2026 | Auth stack and frontend auth service source |
| 4 - 5 | List the S3, DynamoDB, Cognito, and Rekognition operations used by each Lambda and compare them with the grants declared in CDK. | 06/17/2026 | 06/18/2026 | `infrastructure/lib/stacks/api-stack.ts` |
| 5 | Verify that grants for `ApiHandler`, `ImageProcessor`, and `AIAnalyzer` cover the batch/query and object-deletion operations called by the handlers; adjust CDK if a mismatch is found. | 06/18/2026 | 06/18/2026 | Backend handlers and API stack |
| 6 | Deploy the stack and inspect the execution roles and inline policies created by CloudFormation in the IAM Console. | 06/19/2026 | 06/19/2026 | AWS CDK/CloudFormation outputs |

### Week 10 Achievements:

* Created the Cognito User Pool and SPA app client without a client secret. TOTP MFA remains configurable by environment.
* Used the `cognito:groups` claim to identify administrators; the frontend treats users outside the `admin` group as regular users.
* Retained `custom:role` in the configuration, but it is not the primary authorization source. No Post Confirmation trigger currently adds new sign-ups to the `user` group automatically.
* Created execution roles and resource grants through CDK instead of maintaining three manually created managed policies. Inspected resource ARNs and granted actions in the IAM Console after deployment.
