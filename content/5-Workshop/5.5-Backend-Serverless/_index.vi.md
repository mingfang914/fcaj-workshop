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

### Tạo role thủ công

Lặp lại quy trình dưới đây cho `ApiHandler`, `ImageProcessor` và `AiAnalyzer`:

1. Mở [IAM Console](https://console.aws.amazon.com/iam/) → **Roles** → **Create role**.
2. Chọn **AWS service**, use case **Lambda**, rồi chọn **Next**.
3. Gắn AWS managed policy `AWSLambdaBasicExecutionRole`.
4. Đặt tên lần lượt `SmartImage-ApiHandlerRole-staging`, `SmartImage-ImageProcessorRole-staging` và `SmartImage-AiAnalyzerRole-staging`.
5. Tạo role, mở role vừa tạo và chọn **Add permissions** → **Create inline policy**.
6. Chọn tab **JSON**, thêm các action ở bảng dưới và thay ARN mẫu bằng ARN thật của tài nguyên `staging`.
7. Chọn **Next**, đặt tên policy theo function, rồi chọn **Create policy**.

Mỗi Lambda dùng một execution role riêng, có `AWSLambdaBasicExecutionRole` và các quyền theo tài nguyên `staging`:

| Lambda | Quyền chính |
|---|---|
| `ApiHandler` | S3 Get/Put/Delete; DynamoDB Get/Put/Update/Delete/Query/Scan/BatchGet/BatchWrite trên ba bảng và index; `cognito-idp:AdminUpdateUserAttributes` trên User Pool |
| `ImageProcessor` | S3 Get trên raw bucket, Put trên processed bucket; DynamoDB read/write/query trên bảng `Images`; SQS SendMessage cho DLQ |
| `AiAnalyzer` | S3 Get trên raw bucket; DynamoDB stream read và read/write/batch trên bảng `Images`; Rekognition DetectLabels/DetectModerationLabels; SQS SendMessage cho DLQ |

![Gắn custom policy vào Lambda role trong luồng Console](/images/5-Workshop/5.5-Backend-Serverless/iam_roles_setup.png)

> **Khác với CDK:** CDK tạo role và resource grants tự động. Custom managed policy trong ảnh chỉ là cách cấu hình thủ công tương đương. Resource ARN cần giới hạn theo bucket, table, index, stream, queue và User Pool thực tế.

Khi viết policy JSON, cần bao gồm cả ARN bảng và `table/<name>/index/*` cho thao tác Query trên GSI. Với stream, dùng ARN có dạng `table/<name>/stream/*`; với object S3, ARN kết thúc bằng `/*`. Không dùng `Resource: "*"` cho S3, DynamoDB hoặc Cognito chỉ để làm cho bài lab chạy.

### Tạo hai Dead Letter Queues

1. Mở [Amazon SQS Console](https://console.aws.amazon.com/sqs/) → **Create queue**.
2. Chọn queue type **Standard**.
3. Tạo `SmartImage-ImageProcessorDlq-staging`, giữ encryption mặc định và đặt message retention là 14 ngày.
4. Lặp lại để tạo `SmartImage-AiAnalyzerDlq-staging`.
5. Ghi lại ARN của hai queue để cấu hình quyền `sqs:SendMessage` và on-failure destination.

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

### Tạo function trên Console

Với mỗi Lambda nghiệp vụ:

1. Mở [AWS Lambda Console](https://console.aws.amazon.com/lambda/) → **Functions** → **Create function**.
2. Chọn **Author from scratch**.
3. Nhập đúng function name trong bảng trên.
4. Chọn Node.js 22.x hoặc runtime Node.js đang được AWS hỗ trợ và đã được kiểm thử với package.
5. Ở **Architecture**, chọn `arm64`.
6. Trong **Change default execution role**, chọn **Use an existing role** và chọn role tương ứng đã tạo.
7. Chọn **Create function**.
8. Mở **Configuration** → **General configuration** → **Edit**, đặt memory và timeout theo bảng.
9. Riêng ImageProcessor, mở **Ephemeral storage** và đặt `1024 MB`.
10. Mở **Configuration** → **Environment variables** → **Edit** và nhập các biến của function.

### Deployment package

Không zip trực tiếp mã TypeScript. Build bằng esbuild/`NodejsFunction`; riêng `ImageProcessor` phải chứa Sharp dành cho Linux ARM64. AWS SDK v3 có thể được externalize như cấu hình CDK hoặc bundle có kiểm soát.

Để tạo package tương đương CDK:

1. Từ thư mục dự án, cài dependencies và chạy build/test trước khi đóng gói.
2. Bundle từng entry point bằng esbuild cho platform `node`, architecture ARM64 và runtime mục tiêu.
3. Đảm bảo handler được export đúng tên mà Lambda cấu hình (ví dụ `index.handler`).
4. Với ImageProcessor, cài hoặc bundle binary Sharp tương thích `linux-arm64`; package Sharp của Windows không chạy trên Lambda Linux.
5. Nén **nội dung** thư mục output, không nén thêm một thư mục cha.
6. Trong tab **Code**, chọn **Upload from** → **.zip file**, tải package tương ứng và chọn **Save**.
7. Nếu package vượt giới hạn upload trực tiếp, tải lên S3 hoặc dùng Lambda layer/container image; đây là biến thể tùy chọn, không phải cấu hình CDK hiện tại.

### Biến môi trường

Ba Lambda dùng các biến chung: `IMAGE_TABLE_NAME`, `USER_QUOTA_TABLE_NAME`, `USER_PROFILE_TABLE_NAME`, `RAW_BUCKET_NAME`, `PROCESSED_BUCKET_NAME`, `ENVIRONMENT=staging`, `POWERTOOLS_SERVICE_NAME=SmartImage`, `POWERTOOLS_LOG_LEVEL=DEBUG` và `NODE_OPTIONS=--enable-source-maps`.

Bổ sung:

- `ApiHandler`: `USER_POOL_ID`, `PRESIGNED_URL_EXPIRY=900`.
- `ImageProcessor`: `THUMBNAIL_WIDTH=200`, `THUMBNAIL_HEIGHT=200`, `RESIZED_MAX_WIDTH=1920`, `RESIZED_MAX_HEIGHT=1080`.
- `AiAnalyzer`: bắt buộc có `RAW_BUCKET_NAME` và `IMAGE_TABLE_NAME` để đọc ảnh và cập nhật metadata.

Nhập tên vật lý thật của bucket/table/User Pool thay cho logical name. Sau khi lưu, mở **Configuration** → **Permissions** để xác nhận execution role đúng; sau đó chạy một test event nhỏ hoặc mở log group để chắc chắn function khởi tạo được trước khi gắn trigger.

## 3. S3 Event Notification

1. Mở raw bucket trong Amazon S3 Console.
2. Chọn tab **Properties** và cuộn đến **Event notifications**.
3. Chọn **Create event notification**.
4. Nhập tên `InvokeImageProcessor-staging`.
5. Nhập prefix `users/`; để suffix trống.
6. Ở **Event types**, chọn **All object create events**.
7. Ở **Destination**, chọn **Lambda function** và chọn `SmartImage-ImageProcessor-staging`.
8. Chọn **Save changes**. S3 sẽ thêm resource-based permission để được gọi Lambda.

Cấu hình cần đạt:

- Event: **All object create events**.
- Prefix: `users/`.
- Destination: `SmartImage-ImageProcessor-staging`.
- Không thêm suffix nếu muốn để Lambda thực hiện validation định dạng.

![Destination của S3 Event Notification](/images/5-Workshop/5.5-Backend-Serverless/s3_trigger_setup.png)

Ảnh chỉ minh họa destination; event type và prefix phải được nhập theo danh sách trên.

## 4. DynamoDB Stream event source

1. Xác nhận stream của `SmartImage-Images-staging` đã bật với **New and old images**.
2. Mở DynamoDB table → **Exports and streams** → **DynamoDB stream details**.
3. Chọn **Create trigger** hoặc mở Lambda `SmartImage-AiAnalyzer-staging` → **Add trigger** → **DynamoDB**.
4. Chọn stream ARN của bảng Images.
5. Đặt **Batch size** là `10` và bật trigger.
6. Chọn **Add/Create trigger**.

![Tạo DynamoDB trigger với batch size 10](/images/5-Workshop/5.5-Backend-Serverless/dynamodb_trigger_setup.png)

Sau khi tạo, mở event source mapping trong Lambda để kiểm tra/cấu hình:

- Starting position: `TRIM_HORIZON`.
- Maximum retry attempts: `3`.
- On-failure destination: SQS queue `SmartImage-AiAnalyzerDlq-staging`.
- Trigger enabled.

Ảnh chỉ thể hiện form tạo trigger ban đầu; retry và destination được CDK khai báo thêm.

Nếu Console không hiển thị retry và destination trong form DynamoDB, mở Lambda → **Configuration** → **Triggers**, chọn event source mapping rồi **Edit**. Cấp quyền đọc stream cho role trước khi bật mapping; nếu không, trạng thái trigger sẽ báo lỗi.

## 5. API Gateway và Cognito Authorizer

### Tạo REST API

1. Mở [API Gateway Console](https://console.aws.amazon.com/apigateway/) → **Create API**.
2. Ở **REST API**, chọn **Build**. Không chọn HTTP API vì CDK dùng REST API v1.
3. Chọn **New API**, nhập tên `SmartImage-API-staging`, endpoint type **Regional**, rồi chọn **Create API**.

### Tạo Cognito authorizer

1. Trong API vừa tạo, chọn **Authorizers** → **Create authorizer**.
2. Nhập tên `CognitoAuth`, type **Cognito**.
3. Chọn Region `ap-southeast-1` và User Pool `SmartImage-UserPool-staging`.
4. Ở **Token source**, nhập `Authorization`; để token validation trống.
5. Chọn **Create authorizer**.

![Cognito User Pool Authorizer trên API Gateway](/images/5-Workshop/5.5-Backend-Serverless/api_gateway_authorizer.png)

### Tạo resources và methods

1. Chọn **Resources** → **Create resource**, bắt đầu với resource `/v1`.
2. Tạo các resource con theo đúng cây đường dẫn. Với `{imageId}`, giữ dấu ngoặc nhọn để API Gateway nhận đây là path parameter.
3. Trên mỗi resource, chọn **Create method**, chọn HTTP method, integration type **Lambda function**, bật Lambda proxy integration và chọn `SmartImage-ApiHandler-staging`.
4. Gắn authorizer theo bảng sau:

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
| `GET` | `/v1/admin/moderation` | `CognitoAuth` và kiểm tra group trong Lambda |
| `POST` | `/v1/admin/moderation/{imageId}` | `CognitoAuth` và kiểm tra group trong Lambda |

5. Với `PATCH`/`POST`, bật request body validator tương đương CDK. Bật CORS trên các resource được frontend gọi; cho phép origin Amplify, headers `Content-Type,Authorization` và các methods thực tế.
6. Chọn **Deploy API**, tạo stage name `dev`, rồi deploy.
7. Sao chép invoke URL có dạng `https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/dev`.
8. Gọi `GET /v1/images/public` không token để kiểm tra route public; gọi route protected không token phải nhận `401 Unauthorized`.

Trong CDK hiện tại, environment `staging` được ánh xạ sang API Gateway stage `dev`; production dùng stage `prod`.

> CDK hiện còn khai báo `/v1/auth/signup`, `/login` và `/refresh`, nhưng router Lambda không triển khai các handler tương ứng; frontend xác thực trực tiếp với Cognito. Không dùng ba route này trong Console walkthrough.

## 6. WAF và monitoring

CDK còn tạo WAF Web ACL, SQS DLQs, API access log group, CloudWatch dashboard, alarms và SNS topic. Có thể quan sát các tài nguyên này trên Console; không cần cấu hình lại thủ công để hoàn thành phần minh họa.

Nếu muốn dựng thủ công để học thêm, thực hiện sau khi API hoạt động:

1. Tạo CloudWatch log group `/aws/apigateway/SmartImage-staging` và bật access logging trên stage `dev`.
2. Tạo SNS topic `SmartImage-Alarms-staging`, thêm email subscription và xác nhận email.
3. Tạo dashboard `SmartImage-staging-Operations` cùng alarm Errors/Throttles cho ba Lambda và 5XX/latency cho API.
4. Tạo WAF Web ACL loại Regional trong `ap-southeast-1`, thêm AWS managed rules và rate-based rule, rồi associate với API Gateway stage `dev`.
5. Ghi nhận WAF và monitoring là phần mở rộng Console; cấu hình chính xác và đầy đủ vẫn nằm trong CDK stack.
