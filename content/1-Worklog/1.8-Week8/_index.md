---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives:

* Study Serverless computing theory and AWS Lambda execution models.
* Build event-driven automated workflows triggered by storage actions.
* Configure REST APIs using API Gateway as the entry point for serverless backends.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | Study Serverless computing: benefits, AWS Lambda execution limits, and pay-per-execution pricing models. | 06/01/2026 | 06/01/2026 | https://cloudjourney.awsstudygroup.com/ |
| 2 - 3 | Write a basic Node.js 22.x Lambda function. Configure an execution role limited to reading objects from the S3 bucket used by the lab. | 06/01/2026 | 06/02/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Configure S3 Event Notifications on a source bucket to invoke the Lambda function automatically upon file upload. | 06/03/2026 | 06/03/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Study Amazon API Gateway: REST vs HTTP APIs, Resources, HTTP Methods (GET, POST), Stages, and Deployments. | 06/04/2026 | 06/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 - 6 | Practice: Build a REST API in API Gateway, link a GET method to the Lambda function, deploy to stage `dev`, and test using curl. | 06/04/2026 | 06/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Week 8 Achievements:

* Created a Node.js 22.x Lambda function with an execution role limited to object reads from the lab bucket.
* Configured an S3 Event Notification and verified processing after an object upload; the handler was treated as idempotent because S3 can deliver an event more than once.
* Created a REST API GET method using Lambda Proxy Integration.
* Deployed the `dev` stage, called the endpoint with `curl`, and inspected the status code and response body.
