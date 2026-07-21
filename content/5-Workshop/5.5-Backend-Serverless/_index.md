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

### Create the roles manually

Repeat this procedure for `ApiHandler`, `ImageProcessor`, and `AiAnalyzer`:

1. Open the [IAM Console](https://console.aws.amazon.com/iam/) → **Roles** → **Create role**.
2. Select **AWS service**, use case **Lambda**, and choose **Next**.
3. Attach the AWS managed policy `AWSLambdaBasicExecutionRole`.
4. Name the roles `SmartImage-ApiHandlerRole-staging`, `SmartImage-ImageProcessorRole-staging`, and `SmartImage-AiAnalyzerRole-staging`.
5. Create the role, open it, and choose **Add permissions** → **Create inline policy**.
6. Select **JSON**, add the actions in the table below, and replace example ARNs with the actual `staging` resources.
7. Choose **Next**, name the policy for the function, and choose **Create policy**.

Each Lambda has a separate execution role with `AWSLambdaBasicExecutionRole` and resource-scoped `staging` permissions:

| Lambda | Main permissions |
|---|---|
| `ApiHandler` | S3 Get/Put/Delete; DynamoDB Get/Put/Update/Delete/Query/Scan/BatchGet/BatchWrite on three tables and indexes; `cognito-idp:AdminUpdateUserAttributes` on the User Pool |
| `ImageProcessor` | S3 Get on the raw bucket, Put on the processed bucket; DynamoDB read/write/query on `Images`; SQS SendMessage for its DLQ |
| `AiAnalyzer` | S3 Get on the raw bucket; DynamoDB stream read and table read/write/batch; Rekognition DetectLabels/DetectModerationLabels; SQS SendMessage for its DLQ |

![Attach a custom policy to a Lambda role in the Console path](/images/5-Workshop/5.5-Backend-Serverless/iam_roles_setup.png)

> **Difference from CDK:** CDK creates roles and resource grants automatically. The custom managed policy in the screenshot is only a manual equivalent. Restrict resource ARNs to the actual buckets, tables, indexes, streams, queues, and User Pool.

In policy JSON, include both the table ARN and `table/<name>/index/*` for GSI queries. Use `table/<name>/stream/*` for streams and end S3 object ARNs with `/*`. Do not use `Resource: "*"` for S3, DynamoDB, or Cognito simply to make the lab work.

### Create two dead-letter queues

1. Open the [Amazon SQS Console](https://console.aws.amazon.com/sqs/) → **Create queue**.
2. Select **Standard**.
3. Create `SmartImage-ImageProcessorDlq-staging`, keep default encryption, and set message retention to 14 days.
4. Repeat for `SmartImage-AiAnalyzerDlq-staging`.
5. Record both queue ARNs for `sqs:SendMessage` permissions and on-failure destinations.

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

### Create functions in the Console

For each business function:

1. Open the [AWS Lambda Console](https://console.aws.amazon.com/lambda/) → **Functions** → **Create function**.
2. Select **Author from scratch**.
3. Enter the exact function name from the table above.
4. Select Node.js 22.x or another currently supported Node.js runtime tested with the package.
5. Under **Architecture**, select `arm64`.
6. Expand **Change default execution role**, select **Use an existing role**, and choose the matching role.
7. Choose **Create function**.
8. Open **Configuration** → **General configuration** → **Edit** and set memory and timeout from the table.
9. For ImageProcessor, set **Ephemeral storage** to `1024 MB`.
10. Open **Configuration** → **Environment variables** → **Edit** and enter the function values.

### Deployment package

Do not zip raw TypeScript source. Build with esbuild/`NodejsFunction`; `ImageProcessor` must include Sharp compiled for Linux ARM64. AWS SDK v3 can be externalized as in CDK or bundled deliberately.

To create a package equivalent to CDK:

1. Install dependencies and run the project build/tests before packaging.
2. Bundle each entry point with esbuild for the `node` platform, ARM64 architecture, and target runtime.
3. Verify that the configured handler is exported under the expected name, such as `index.handler`.
4. For ImageProcessor, install or bundle a `linux-arm64` Sharp binary; a Windows Sharp package will not run on Lambda Linux.
5. Zip the **contents** of the output directory, not an extra parent directory.
6. On the **Code** tab, choose **Upload from** → **.zip file**, upload the matching package, and choose **Save**.
7. For a package above the direct-upload limit, upload through S3 or use a layer/container image. That is an optional variation, not the current CDK configuration.

### Environment variables

All three functions use `IMAGE_TABLE_NAME`, `USER_QUOTA_TABLE_NAME`, `USER_PROFILE_TABLE_NAME`, `RAW_BUCKET_NAME`, `PROCESSED_BUCKET_NAME`, `ENVIRONMENT=staging`, `POWERTOOLS_SERVICE_NAME=SmartImage`, `POWERTOOLS_LOG_LEVEL=DEBUG`, and `NODE_OPTIONS=--enable-source-maps`.

Additional values:

- `ApiHandler`: `USER_POOL_ID`, `PRESIGNED_URL_EXPIRY=900`.
- `ImageProcessor`: `THUMBNAIL_WIDTH=200`, `THUMBNAIL_HEIGHT=200`, `RESIZED_MAX_WIDTH=1920`, `RESIZED_MAX_HEIGHT=1080`.
- `AiAnalyzer`: `RAW_BUCKET_NAME` and `IMAGE_TABLE_NAME` are required for image reads and metadata updates.

Use actual physical resource names rather than logical names. After saving, open **Configuration** → **Permissions** and verify the execution role. Run a small test event or inspect the log group to confirm that the function initializes before adding triggers.

## 3. S3 Event Notification

1. Open the raw bucket in the Amazon S3 Console.
2. Choose **Properties** and scroll to **Event notifications**.
3. Choose **Create event notification**.
4. Enter `InvokeImageProcessor-staging` as the name.
5. Enter prefix `users/` and leave suffix empty.
6. Under **Event types**, select **All object create events**.
7. Under **Destination**, select **Lambda function** and `SmartImage-ImageProcessor-staging`.
8. Choose **Save changes**. S3 adds the resource-based permission required to invoke Lambda.

The result must be:

- Event: **All object create events**.
- Prefix: `users/`.
- Destination: `SmartImage-ImageProcessor-staging`.
- Do not add a suffix when Lambda is responsible for format validation.

![S3 Event Notification destination](/images/5-Workshop/5.5-Backend-Serverless/s3_trigger_setup.png)

The screenshot illustrates the destination only; enter the event type and prefix listed above.

## 4. DynamoDB Stream event source

1. Verify that `SmartImage-Images-staging` has a **New and old images** stream.
2. Open the table → **Exports and streams** → **DynamoDB stream details**.
3. Choose **Create trigger**, or open `SmartImage-AiAnalyzer-staging` → **Add trigger** → **DynamoDB**.
4. Select the Images table stream ARN.
5. Set **Batch size** to `10` and enable the trigger.
6. Choose **Add/Create trigger**.

![Create a DynamoDB trigger with batch size 10](/images/5-Workshop/5.5-Backend-Serverless/dynamodb_trigger_setup.png)

After creation, open the event source mapping in Lambda and verify/configure:

- Starting position: `TRIM_HORIZON`.
- Maximum retry attempts: `3`.
- On-failure destination: `SmartImage-AiAnalyzerDlq-staging` SQS queue.
- Trigger enabled.

The screenshot shows only the initial trigger form; CDK also declares retry and destination settings.

If retry and destination are absent from the DynamoDB form, open Lambda → **Configuration** → **Triggers**, select the event source mapping, and choose **Edit**. Grant stream-read permission to the role before enabling the mapping, or the trigger will enter an error state.

## 5. API Gateway and Cognito Authorizer

### Create the REST API

1. Open the [API Gateway Console](https://console.aws.amazon.com/apigateway/) → **Create API**.
2. Under **REST API**, choose **Build**. Do not choose HTTP API because CDK uses REST API v1.
3. Select **New API**, enter `SmartImage-API-staging`, choose **Regional**, and choose **Create API**.

### Create the Cognito authorizer

1. In the new API, choose **Authorizers** → **Create authorizer**.
2. Enter `CognitoAuth` and select type **Cognito**.
3. Select Region `ap-southeast-1` and User Pool `SmartImage-UserPool-staging`.
4. Enter `Authorization` as the **Token source** and leave token validation empty.
5. Choose **Create authorizer**.

![Cognito User Pool Authorizer in API Gateway](/images/5-Workshop/5.5-Backend-Serverless/api_gateway_authorizer.png)

### Create resources and methods

1. Choose **Resources** → **Create resource** and begin with `/v1`.
2. Create the nested resource tree. Preserve braces in `{imageId}` so API Gateway treats it as a path parameter.
3. For each resource, choose **Create method**, select the HTTP method, use **Lambda function** with Lambda proxy integration, and select `SmartImage-ApiHandler-staging`.
4. Apply authorization as follows:

| Method | Path | Authorization |
|---|---|---|
| `GET`, `PATCH` | `/v1/profile` | `CognitoAuth` |
| `POST` | `/v1/profile/avatar/presigned-url` | `CognitoAuth` |
| `GET` | `/v1/images` | `CognitoAuth` |
| `POST` | `/v1/images/presigned-url` | `CognitoAuth` |
| `GET` | `/v1/images/public` | `NONE` |
| `DELETE` | `/v1/images/bulk` | `CognitoAuth` |
| `GET` | `/v1/images/search` | `CognitoAuth` |
| `GET`, `DELETE`, `PATCH` | `/v1/images/{imageId}` | `CognitoAuth` |
| `GET` | `/v1/images/{imageId}/download` | `CognitoAuth` |
| `GET` | `/v1/admin/moderation` | `CognitoAuth` plus group check in Lambda |
| `POST` | `/v1/admin/moderation/{imageId}` | `CognitoAuth` plus group check in Lambda |

5. Add a request-body validator to `PATCH`/`POST` methods to mirror CDK. Enable CORS on resources called by the frontend; allow the Amplify origin, `Content-Type,Authorization` headers, and only the methods used.
6. Choose **Deploy API**, create stage `dev`, and deploy.
7. Copy the invoke URL: `https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/dev`.
8. Call `GET /v1/images/public` without a token to verify the public route; a protected route without a token should return `401 Unauthorized`.

In the current CDK code, the `staging` environment maps to API Gateway stage `dev`; production uses `prod`.

> CDK currently also declares `/v1/auth/signup`, `/login`, and `/refresh`, but the Lambda router has no matching handlers; the frontend authenticates directly with Cognito. Do not use those three routes in the Console walkthrough.

## 6. WAF and monitoring

CDK also creates a WAF Web ACL, SQS DLQs, an API access log group, a CloudWatch dashboard, alarms, and an SNS topic. These resources can be inspected in the Console and do not need to be recreated for the visual walkthrough.

For an optional manual extension after the API works:

1. Create `/aws/apigateway/SmartImage-staging` and enable access logging on stage `dev`.
2. Create SNS topic `SmartImage-Alarms-staging`, add an email subscription, and confirm the email.
3. Create dashboard `SmartImage-staging-Operations` and Errors/Throttles alarms for the Lambdas plus API 5XX/latency alarms.
4. Create a Regional WAF Web ACL in `ap-southeast-1`, add AWS managed rules and a rate-based rule, and associate it with API stage `dev`.
5. Treat WAF and monitoring as a Console extension; the complete authoritative configuration remains in CDK.
