---
title: "Testing and Verification"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

# Test Smart Image Platform

The following steps use the CDK `staging` deployment. Substitute equivalent names when resources were created manually.

## 1. User authentication

1. Open the Amplify branch URL or a custom domain pointing to Amplify.
2. Sign up with an email, full name, and a password satisfying the configured policy.
3. Enter the email confirmation code.
4. Sign in and verify that the ID token belongs to the expected User Pool; add an administrator test account to the `admin` group.

![Screen displayed after sign-up confirmation](/images/5-Workshop/5.7-Testing-Validation/react_app_login.png)

## 2. Image upload and processing

1. Select a valid JPEG or PNG in Upload Images.
2. Verify that presigned URL generation succeeds and that the browser PUTs the object directly to the raw bucket.
3. Follow the image status in the UI; processing is asynchronous, so poll until a terminal state is reached.
4. In the processed bucket, inspect the `resized/` and `thumbnails/` prefixes under `users/<user-id>/`.

![Output folders in the staging processed bucket](/images/5-Workshop/5.7-Testing-Validation/s3_processed_objects.png)

## 3. Metadata and Rekognition

Open `SmartImage-Images-staging`, use **Explore items**, and inspect the image item:

- `PK`/`SK` follow the access pattern.
- `status` reflects pipeline progress.
- `thumbnailKey`/`resizedKey` point to the processed bucket.
- `aiTags`, moderation labels/status, and EXIF are present after their respective steps complete.

![Inspect aiTags on a DynamoDB item](/images/5-Workshop/5.7-Testing-Validation/dynamodb_item_tags.png)

Use the screen for observation only; do not edit `aiTags` manually.

## 4. Frontend gallery

Open My Gallery, select a processed image, and inspect its preview, status, metadata, and AI labels. Community Gallery calls `GET /v1/images/public`; My Gallery uses a Cognito-protected route.

![A COMPLETED image with Rekognition labels](/images/5-Workshop/5.7-Testing-Validation/react_app_dashboard.png)

## 5. Logs, metrics, and alarms

Inspect:

- Lambda log groups corresponding to `SmartImage-ApiHandler-staging`, `SmartImage-ImageProcessor-staging`, and `SmartImage-AiAnalyzer-staging`.
- API access log group `/aws/apigateway/SmartImage-staging`.
- Dashboard `SmartImage-staging-Operations`.
- SNS topic `SmartImage-Alarms-staging`; confirm the email subscription before expecting notifications.
- SQS queues `SmartImage-ImageProcessorDlq-staging` and `SmartImage-AiAnalyzerDlq-staging`.

The Lambda Errors alarm uses a five-minute `Sum`, threshold 5, and **GreaterThanThreshold**; at least six errors in one period are required to enter ALARM.

## 6. Failure testing

- Use an unsupported extension to verify frontend validation. Rejection occurs before upload and therefore does not create a Lambda error.
- To exercise backend validation, upload an object with an allowed image extension but invalid content directly to the `staging` raw bucket, then inspect logs and item status.
- `ImageProcessor` and `AiAnalyzer` currently catch errors without rethrowing in some paths. When Lambda treats the invocation as successful, the `Errors` metric, retries, and DLQ do not behave like an unhandled failure.
- Report SNS/DLQ validation as successful only after correcting error propagation or running a separate controlled alarm test.

> Do not generate repeated failures in production. Remove test objects and items after validation.
