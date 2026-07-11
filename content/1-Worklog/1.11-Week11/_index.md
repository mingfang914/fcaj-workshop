---
title: "Week 11 Worklog"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Deploy backend code packages to the three Lambda functions using optimized runtimes.
* Wire storage notifications and database streams to trigger serverless executions automatically.
* Configure API Gateway REST APIs secured via Cognito Authorizers.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Deploy Lambda functions (`ApiHandler`, `ImageProcessor`, `AIAnalyzer`) using Node.js 22.x runtime, ARM64 architecture, and attach execution roles. | 06/22/2026 | 06/22/2026 |  |
| 3 - 5 | Zip and upload deployment packages (including sharp for image resizing and AWS SDK modules), and configure environment variables connecting to tables and S3 buckets. | 06/23/2026 | 06/25/2026 |  |
| 5 | Configure S3 Event Notifications to trigger `ImageProcessor` (prefix filter: `users/`) and enable DynamoDB stream triggers for `AIAnalyzer` (batch size: 1). | 06/25/2026 | 06/25/2026 |  |
| 6 | Create REST API `SmartImage-API` in API Gateway and configure the Cognito Authorizer. | 06/26/2026 | 06/26/2026 |  |
| 6 | Practice: Create proxy resource `{proxy+}` with CORS, configure ANY method integration to `ApiHandler` via proxy, and deploy to stage `dev`. | 06/26/2026 | 06/26/2026 |  |

### Week 11 Achievements:

* Deployed and validated three serverless Lambda functions on the ARM64 architecture, decreasing execution latencies and lowering resource costs.
* Configured S3 Event Notifications targeting the `users/` prefix to invoke `ImageProcessor`, automating image compression and thumbnail creation via Sharp.
* Enabled DynamoDB Streams with New Image view type to trigger `AIAnalyzer`, integrating Amazon Rekognition for automated tagging and content moderation.
* Built a REST API on API Gateway with a wildcard `{proxy+}` resource and `ANY` method linked to the `ApiHandler` using Lambda Proxy Integration with CORS enabled.
* Integrated a Cognito Authorizer (`CognitoAuth`) to secure the API Gateway endpoints, rejecting requests without valid ID tokens.
