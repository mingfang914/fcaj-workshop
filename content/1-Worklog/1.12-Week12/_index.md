---
title: "Week 12 Worklog"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Week 12 Objectives:

* Host the React client application on AWS Amplify connected to Git.
* Perform end-to-end integration tests spanning Cognito user signup, login, image upload, and tag rendering.
* Clean up all provisioned cloud resources to prevent extra charges.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Connect the GitHub repository branch to AWS Amplify Hosting and configure the build template `amplify.yml`. | 06/29/2026 | 06/29/2026 |  |
| 3 | Set up environment variables (Cognito client and user pool details, API URL) in Amplify and trigger deployment. | 06/30/2026 | 06/30/2026 |  |
| 4 | Perform E2E tests: register user, verify login, upload files via S3 Presigned URL, check thumbnail generation, and verify AI tags. | 07/01/2026 | 07/01/2026 |  |
| 5 | Test system failure by uploading unsupported files, inspect error logs in CloudWatch Logs, check Dashboard metrics, and verify SNS email notifications. | 07/02/2026 | 07/02/2026 |  |
| 6 | Practice: Empty S3 buckets and run `cdk destroy --all` to tear down all provisioned resources and prevent extra charges. | 07/03/2026 | 07/03/2026 |  |

### Week 12 Achievements:

* Connected the GitHub repository to AWS Amplify hosting and configured `amplify.yml` to support automated building of the React client under the npm workspace monorepo.
* Deployed the React frontend application, configuring client environment variables (`VITE_API_URL`, `VITE_USER_POOL_ID`, `VITE_CLIENT_ID`) to link to API Gateway.
* Verified the end-to-end user flow: user registration, email verification code delivery, login, uploading images via secure S3 Presigned URLs, thumbnail rendering, and AI label ingestion.
* Executed failure scenario tests by uploading unsupported files, successfully tracing stack errors in CloudWatch Logs (`/aws/lambda/SmartImage-ImageProcessor`), and receiving email alerts via SNS.
* Executed cleanup procedures by emptying S3 buckets and running `cdk destroy --all` to completely tear down the stack, avoiding ongoing charges.
