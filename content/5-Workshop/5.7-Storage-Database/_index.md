---
title: "Console - Storage and Database"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Storage and Database in the AWS Console (optional)

> This section illustrates equivalent Console settings. Skip all resource-creation steps after deploying the `staging` CDK stacks.

## 1. Amazon S3

The system uses two private buckets to separate originals from processed output. For a manual exercise, use unique names with a `staging` suffix; physical names do not have to match CDK-generated names exactly.

### Raw bucket

1. Open the Amazon S3 Console and choose **Create bucket**.
2. Select `ap-southeast-1`, **General purpose**, ACLs disabled, and **Block all public access**.
3. Select **SSE-S3**; leave S3 Bucket Key **Disabled** because it applies to SSE-KMS.
4. Enable **Bucket Versioning**.
5. After creation, add CORS for `PUT`/`POST`, the required headers, and the frontend origin.
6. Add lifecycle transitions to Standard-IA after 90 days and Glacier Flexible Retrieval after 365 days, plus the lab's noncurrent-version expiration policy.

![Initial settings for the staging raw bucket](/images/5-Workshop/5.3-Storage-Database/s3_raw_setup.png)

### Processed bucket

1. Create the second bucket with ACLs disabled, Block all public access, and SSE-S3.
2. Leave versioning disabled to match the current CDK stack.
3. Leave S3 Bucket Key **Disabled**.
4. After creation, add CORS for `GET`/`HEAD` and a lifecycle rule that aborts incomplete multipart uploads after seven days.

![Processed bucket versioning and encryption](/images/5-Workshop/5.3-Storage-Database/s3_processed_setup.png)

> Both buckets remain private. The frontend uses presigned URLs; do not enable static website hosting or a public-read policy.

## 2. Amazon DynamoDB

Create three tables in **On-demand** mode with default encryption and point-in-time recovery enabled:

| Example table | Partition key | Sort key | Purpose |
|---|---|---|---|
| `SmartImage-Images-staging` | `PK` (String) | `SK` (String) | Image metadata and related access patterns |
| `SmartImage-UserQuotas-staging` | `PK` (String) | `SK` (String) | Upload and storage quotas |
| `SmartImage-UserProfiles-staging` | `PK` (String) | `SK` (String) | User profiles |

![Primary key definition for the Images table](/images/5-Workshop/5.3-Storage-Database/dynamodb_table_setup.png)

### Images table stream

1. Open the `Images` table and choose **Exports and streams**.
2. Enable DynamoDB Streams with **New and old images** view type.
3. Do not create the Lambda trigger yet; it is illustrated in the Backend section.

### Global Secondary Indexes

Create two GSIs on `Images`:

- `GSI1-TagIndex-v2`: String keys `GSI1PK` and `GSI1SK`; `INCLUDE` projection with `imageId`, `userId`, `thumbnailKey`, `originalFilename`, `createdAt`, `moderationStatus`, `imagePK`, and `imageSK`.
- `GSI2-ModerationIndex`: String keys `GSI2PK` and `GSI2SK`; `INCLUDE` projection with `imageId`, `userId`, `originalKey`, `moderationLabels`, `thumbnailKey`, and `createdAt`.

![GSI1 key schema](/images/5-Workshop/5.3-Storage-Database/dynamodb_gsi_setup.png)

> The screenshot illustrates only the GSI1 key schema. Projection `ALL` may be used for a shorter Console exercise, but it is optional and differs from CDK.
