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
* Remove test resources and inspect resources retained by removal policies.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Connect the GitHub repository branch to AWS Amplify Hosting and configure the build template `amplify.yml`. | 06/29/2026 | 06/29/2026 |  |
| 2 - 3 | Set up environment variables (Cognito client and user pool details, API URL) in Amplify and trigger deployment. | 06/29/2026 | 06/30/2026 |  |
| 4 - 5 | Perform E2E tests: register user, verify login, upload files via S3 Presigned URL, check thumbnail generation, and verify AI tags. | 07/01/2026 | 07/02/2026 |  |
| 5 | Check frontend validation with unsupported formats; use a file with an allowed extension but invalid image data to inspect backend logs and failure status. Compare the observed behavior with the CloudWatch Alarm and DLQ design. | 07/02/2026 | 07/02/2026 | Frontend uploader, Lambda handlers, and Monitoring stack |
| 6 | Empty the test-environment buckets, run `cdk destroy --all`, and inspect S3, DynamoDB, CloudWatch Logs, SQS, SNS, and WAF for retained resources. | 07/03/2026 | 07/03/2026 | CDK stacks and AWS Billing Console |

### Week 12 Achievements:

* Connected the repository to Amplify Hosting and used `amplify.yml` to build the React client within the npm workspace monorepo.
* Configured `VITE_API_URL`, `VITE_USER_POOL_ID`, and `VITE_CLIENT_ID`; verified that the frontend called the API for the deployed environment.
* Checked registration, email confirmation, login, presigned URL generation, JPEG/PNG upload, and status polling until image metadata and Rekognition labels were returned.
* The frontend rejected unsupported formats before upload. The processing Lambdas currently catch errors without rethrowing them, so an invocation may not increment the `Errors` metric, trigger retries or the DLQ, or send the SNS alarm described by the original scenario. Alert delivery is recorded only after correcting error propagation or running an independent alarm test.
* Ran `cdk destroy --all` for test resources and inspected retained resources manually. No zero-cost claim was made because production resources may use `RETAIN`, and logs, data, or resources outside the stack may continue to incur charges.
