---
title: "Console - Backend Serverless"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Backend Serverless trên AWS Console (tùy chọn)

> Đây là bản đồ cấu hình Console, không phải phương thức deploy chính. Nếu API stack đã tồn tại, chỉ dùng Console để quan sát; không tạo lại Lambda, trigger hoặc API cùng tên.

## 1. Execution roles và quyền

Mỗi Lambda dùng một execution role riêng, có `AWSLambdaBasicExecutionRole` và các quyền theo tài nguyên `staging`:

| Lambda | Quyền chính |
|---|---|
| `ApiHandler` | S3 Get/Put/Delete; DynamoDB Get/Put/Update/Delete/Query/Scan/BatchGet/BatchWrite trên ba bảng và index; `cognito-idp:AdminUpdateUserAttributes` trên User Pool |
| `ImageProcessor` | S3 Get trên raw bucket, Put trên processed bucket; DynamoDB read/write/query trên bảng `Images`; SQS SendMessage cho DLQ |
| `AiAnalyzer` | S3 Get trên raw bucket; DynamoDB stream read và read/write/batch trên bảng `Images`; Rekognition DetectLabels/DetectModerationLabels; SQS SendMessage cho DLQ |

![Gắn custom policy vào Lambda role trong luồng Console](/images/5-Workshop/5.5-Backend-Serverless/iam_roles_setup.png)

> **Khác với CDK:** CDK tạo role và resource grants tự động. Custom managed policy trong ảnh chỉ là cách cấu hình thủ công tương đương. Resource ARN cần giới hạn theo bucket, table, index, stream, queue và User Pool thực tế.

## 2. Lambda functions

Console walkthrough tập trung vào ba Lambda nghiệp vụ:

| Function | Architecture | Memory | Timeout | Temporary storage |
|---|---:|---:|---:|---:|
| `SmartImage-ApiHandler-staging` | ARM64 | 512 MB | 15 giây | Mặc định |
| `SmartImage-ImageProcessor-staging` | ARM64 | 1536 MB | 120 giây | 1024 MB |
| `SmartImage-AiAnalyzer-staging` | ARM64 | 512 MB | 60 giây | Mặc định |

Ảnh deployment hiện tại hiển thị Node.js 20.x vì đó là runtime trong mã CDK tại thời điểm chụp. Khi tạo mới thủ công, chọn Node.js 22.x hoặc runtime còn được AWS hỗ trợ và kiểm thử tương thích trước khi deploy.

![Các Lambda của môi trường staging sau khi deploy CDK](/images/5-Workshop/5.5-Backend-Serverless/lambda_list.png)

Các function có tên dài như `CustomS3AutoDeleteObject`, `BucketNotificationsHandler` và `LogRetention` là provider functions do CDK tạo. `SmartImage-Authorizer-staging` cũng tồn tại nhưng API đang dùng Cognito User Pool Authorizer.

### Deployment package

Không zip trực tiếp mã TypeScript. Build bằng esbuild/`NodejsFunction`; riêng `ImageProcessor` phải chứa Sharp dành cho Linux ARM64. AWS SDK v3 có thể được externalize như cấu hình CDK hoặc bundle có kiểm soát.

### Biến môi trường

Ba Lambda dùng các biến chung: `IMAGE_TABLE_NAME`, `USER_QUOTA_TABLE_NAME`, `USER_PROFILE_TABLE_NAME`, `RAW_BUCKET_NAME`, `PROCESSED_BUCKET_NAME`, `ENVIRONMENT=staging`, `POWERTOOLS_SERVICE_NAME=SmartImage`, `POWERTOOLS_LOG_LEVEL=DEBUG` và `NODE_OPTIONS=--enable-source-maps`.

Bổ sung:

- `ApiHandler`: `USER_POOL_ID`, `PRESIGNED_URL_EXPIRY=900`.
- `ImageProcessor`: `THUMBNAIL_WIDTH=200`, `THUMBNAIL_HEIGHT=200`, `RESIZED_MAX_WIDTH=1920`, `RESIZED_MAX_HEIGHT=1080`.
- `AiAnalyzer`: bắt buộc có `RAW_BUCKET_NAME` và `IMAGE_TABLE_NAME` để đọc ảnh và cập nhật metadata.

## 3. S3 Event Notification

Trên raw bucket, tạo event notification:

- Event: **All object create events**.
- Prefix: `users/`.
- Destination: `SmartImage-ImageProcessor-staging`.
- Không thêm suffix nếu muốn để Lambda thực hiện validation định dạng.

![Destination của S3 Event Notification](/images/5-Workshop/5.5-Backend-Serverless/s3_trigger_setup.png)

Ảnh chỉ minh họa destination; event type và prefix phải được nhập theo danh sách trên.

## 4. DynamoDB Stream event source

Tạo trigger từ stream của bảng `Images` đến `SmartImage-AiAnalyzer-staging` với batch size 10.

![Tạo DynamoDB trigger với batch size 10](/images/5-Workshop/5.5-Backend-Serverless/dynamodb_trigger_setup.png)

Sau khi tạo, mở event source mapping trong Lambda để kiểm tra/cấu hình:

- Starting position: `TRIM_HORIZON`.
- Maximum retry attempts: `3`.
- On-failure destination: SQS queue `SmartImage-AiAnalyzerDlq-staging`.
- Trigger enabled.

Ảnh chỉ thể hiện form tạo trigger ban đầu; retry và destination được CDK khai báo thêm.

## 5. API Gateway và Cognito Authorizer

1. Tạo REST API cho môi trường `staging` và một Cognito authorizer tên `CognitoAuth`.
2. Chọn User Pool `SmartImage-UserPool-staging`; token source là header `Authorization`.

![Cognito User Pool Authorizer trên API Gateway](/images/5-Workshop/5.5-Backend-Serverless/api_gateway_authorizer.png)

3. Tạo Lambda proxy integration tới `SmartImage-ApiHandler-staging`.
4. Tạo các resource/method chính: `/v1/profile`, `/v1/images`, `/v1/images/presigned-url`, `/v1/images/public`, `/v1/images/search`, `/v1/images/{imageId}`, `/v1/images/{imageId}/download`, `/v1/admin/moderation`.
5. Gắn `CognitoAuth` cho các route cần đăng nhập. Giữ `GET /v1/images/public` ở trạng thái public; quyền admin vẫn được kiểm tra thêm trong Lambda bằng `cognito:groups`.
6. Bật CORS cho origin frontend và deploy stage `dev`. Trong CDK hiện tại, environment `staging` được ánh xạ sang API Gateway stage `dev`; production dùng stage `prod`.

> CDK hiện còn khai báo `/v1/auth/signup`, `/login` và `/refresh`, nhưng router Lambda không triển khai các handler tương ứng; frontend xác thực trực tiếp với Cognito. Không dùng ba route này trong Console walkthrough.

## 6. WAF và monitoring

CDK còn tạo WAF Web ACL, SQS DLQs, API access log group, CloudWatch dashboard, alarms và SNS topic. Có thể quan sát các tài nguyên này trên Console; không cần cấu hình lại thủ công để hoàn thành phần minh họa.
