---
title: "Week 11 Worklog"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Deploy three business Lambda functions through AWS CDK and inspect each function's runtime, architecture, memory, and timeout settings.
* Wire storage notifications and database streams to trigger serverless executions automatically.
* Configure API Gateway REST APIs secured via Cognito Authorizers.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Declare `ApiHandler`, `ImageProcessor`, and `AIAnalyzer` in CDK on ARM64; inspect the memory, timeout, temporary storage, and execution role settings for each function. | 06/22/2026 | 06/22/2026 | `infrastructure/lib/stacks/api-stack.ts` |
| 3 - 5 | Bundle TypeScript through the `NodejsFunction` esbuild workflow; package Sharp for Linux ARM64 and configure all S3/DynamoDB environment variables. | 06/23/2026 | 06/25/2026 | API stack and backend handlers |
| 5 | Configure an S3 Event Notification for `ImageProcessor` with the `users/` prefix; configure the `AIAnalyzer` DynamoDB Stream source with batch size 10, three retries, and an SQS DLQ. | 06/25/2026 | 06/25/2026 | `infrastructure/lib/stacks/api-stack.ts` |
| 6 | Declare the REST API and Cognito User Pool Authorizer in CDK; inspect public and authenticated endpoints after deployment. | 06/26/2026 | 06/26/2026 | `infrastructure/lib/stacks/api-stack.ts` |
| 6 | Create explicit resources and methods under `/v1/profile`, `/v1/images`, and `/v1/admin/...`; configure CORS and deploy the environment stage. | 06/26/2026 | 06/26/2026 | API stack and API handler router |

### Week 11 Achievements:

* Deployed three business Lambda functions on ARM64 through CDK. No dedicated benchmark was run, so no quantitative latency or cost-saving claim was recorded.
* Triggered `ImageProcessor` from the raw bucket and bundled Sharp for Linux ARM64 before writing output to the processed bucket.
* Configured `AIAnalyzer` with DynamoDB Stream batch size 10, a maximum of three retries, and an SQS on-failure destination. The function requires `RAW_BUCKET_NAME` in addition to the other table and bucket variables.
* Used explicitly declared API Gateway resources and methods rather than `{proxy+}`/`ANY`. `/v1/images/public` is public; the remaining routes use the Cognito User Pool Authorizer where configured.
* The API stack currently declares Node.js 20.x. Upgrading it to Node.js 22.x remains required for alignment with supported runtimes in 2026; the worklog does not report Node.js 22.x as deployed until the source is updated.
