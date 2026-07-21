---
title: "Console - Serverless Backend"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Serverless Backend in the AWS Console (optional)

> This is a Console configuration map, not the primary deployment method. If the API stack exists, use the Console only for inspection; do not recreate functions, triggers, or APIs with the same names.

## 1. Execution roles and permissions

Each Lambda has a separate execution role with `AWSLambdaBasicExecutionRole` and resource-scoped `staging` permissions:

| Lambda | Main permissions |
|---|---|
| `ApiHandler` | S3 Get/Put/Delete; DynamoDB Get/Put/Update/Delete/Query/Scan/BatchGet/BatchWrite on three tables and indexes; `cognito-idp:AdminUpdateUserAttributes` on the User Pool |
| `ImageProcessor` | S3 Get on the raw bucket, Put on the processed bucket; DynamoDB read/write/query on `Images`; SQS SendMessage for its DLQ |
| `AiAnalyzer` | S3 Get on the raw bucket; DynamoDB stream read and table read/write/batch; Rekognition DetectLabels/DetectModerationLabels; SQS SendMessage for its DLQ |

![Attach a custom policy to a Lambda role in the Console path](/images/5-Workshop/5.5-Backend-Serverless/iam_roles_setup.png)

> **Difference from CDK:** CDK creates roles and resource grants automatically. The custom managed policy in the screenshot is only a manual equivalent. Restrict resource ARNs to the actual buckets, tables, indexes, streams, queues, and User Pool.

## 2. Lambda functions

The Console walkthrough focuses on three business functions:

| Function | Architecture | Memory | Timeout | Temporary storage |
|---|---:|---:|---:|---:|
| `SmartImage-ApiHandler-staging` | ARM64 | 512 MB | 15 seconds | Default |
| `SmartImage-ImageProcessor-staging` | ARM64 | 1536 MB | 120 seconds | 1024 MB |
| `SmartImage-AiAnalyzer-staging` | ARM64 | 512 MB | 60 seconds | Default |

The deployment screenshot shows Node.js 20.x because that is the CDK runtime at capture time. For a newly created manual function, select Node.js 22.x or another currently supported runtime and test compatibility before deployment.

![Lambda functions in staging after CDK deployment](/images/5-Workshop/5.5-Backend-Serverless/lambda_list.png)

Long names such as `CustomS3AutoDeleteObject`, `BucketNotificationsHandler`, and `LogRetention` are CDK provider functions. `SmartImage-Authorizer-staging` also exists, but the API uses the Cognito User Pool Authorizer.

### Deployment package

Do not zip raw TypeScript source. Build with esbuild/`NodejsFunction`; `ImageProcessor` must include Sharp compiled for Linux ARM64. AWS SDK v3 can be externalized as in CDK or bundled deliberately.

### Environment variables

All three functions use `IMAGE_TABLE_NAME`, `USER_QUOTA_TABLE_NAME`, `USER_PROFILE_TABLE_NAME`, `RAW_BUCKET_NAME`, `PROCESSED_BUCKET_NAME`, `ENVIRONMENT=staging`, `POWERTOOLS_SERVICE_NAME=SmartImage`, `POWERTOOLS_LOG_LEVEL=DEBUG`, and `NODE_OPTIONS=--enable-source-maps`.

Additional values:

- `ApiHandler`: `USER_POOL_ID`, `PRESIGNED_URL_EXPIRY=900`.
- `ImageProcessor`: `THUMBNAIL_WIDTH=200`, `THUMBNAIL_HEIGHT=200`, `RESIZED_MAX_WIDTH=1920`, `RESIZED_MAX_HEIGHT=1080`.
- `AiAnalyzer`: `RAW_BUCKET_NAME` and `IMAGE_TABLE_NAME` are required for image reads and metadata updates.

## 3. S3 Event Notification

Create an event notification on the raw bucket:

- Event: **All object create events**.
- Prefix: `users/`.
- Destination: `SmartImage-ImageProcessor-staging`.
- Do not add a suffix when Lambda is responsible for format validation.

![S3 Event Notification destination](/images/5-Workshop/5.5-Backend-Serverless/s3_trigger_setup.png)

The screenshot illustrates the destination only; enter the event type and prefix listed above.

## 4. DynamoDB Stream event source

Create a trigger from the `Images` stream to `SmartImage-AiAnalyzer-staging` with batch size 10.

![Create a DynamoDB trigger with batch size 10](/images/5-Workshop/5.5-Backend-Serverless/dynamodb_trigger_setup.png)

After creation, open the event source mapping in Lambda and verify/configure:

- Starting position: `TRIM_HORIZON`.
- Maximum retry attempts: `3`.
- On-failure destination: `SmartImage-AiAnalyzerDlq-staging` SQS queue.
- Trigger enabled.

The screenshot shows only the initial trigger form; CDK also declares retry and destination settings.

## 5. API Gateway and Cognito Authorizer

1. Create a REST API for `staging` and a Cognito authorizer named `CognitoAuth`.
2. Select `SmartImage-UserPool-staging`; use the `Authorization` header as the token source.

![Cognito User Pool Authorizer in API Gateway](/images/5-Workshop/5.5-Backend-Serverless/api_gateway_authorizer.png)

3. Create a Lambda proxy integration for `SmartImage-ApiHandler-staging`.
4. Create the primary resources and methods under `/v1/profile`, `/v1/images`, `/v1/images/presigned-url`, `/v1/images/public`, `/v1/images/search`, `/v1/images/{imageId}`, `/v1/images/{imageId}/download`, and `/v1/admin/moderation`.
5. Attach `CognitoAuth` to authenticated routes. Keep `GET /v1/images/public` public; Lambda additionally verifies the `cognito:groups` claim for admin operations.
6. Configure CORS for the frontend origin and deploy the `dev` stage. In the current CDK code, the `staging` environment maps to API Gateway stage `dev`; production uses `prod`.

> CDK currently also declares `/v1/auth/signup`, `/login`, and `/refresh`, but the Lambda router has no matching handlers; the frontend authenticates directly with Cognito. Do not use those three routes in the Console walkthrough.

## 6. WAF and monitoring

CDK also creates a WAF Web ACL, SQS DLQs, an API access log group, a CloudWatch dashboard, alarms, and an SNS topic. These resources can be inspected in the Console and do not need to be recreated for the visual walkthrough.
