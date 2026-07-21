---
title: "Console - Storage và Database"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Storage và Database trên AWS Console (tùy chọn)

> Phần này minh họa cấu hình tương đương trên Console. Bỏ qua toàn bộ bước tạo tài nguyên nếu các stack CDK `staging` đã được deploy.

## 1. Amazon S3

Hệ thống sử dụng hai bucket private để tách ảnh gốc và kết quả xử lý. Khi tạo thủ công, dùng tên duy nhất có hậu tố `staging`; tên vật lý không cần giống chính xác tên do CDK tạo.

### Raw bucket

1. Mở Amazon S3 Console và chọn **Create bucket**.
2. Chọn Region `ap-southeast-1`, **General purpose**, ACL disabled và **Block all public access**.
3. Chọn **SSE-S3**; S3 Bucket Key để **Disable** vì chỉ áp dụng cho SSE-KMS.
4. Bật **Bucket Versioning**.
5. Sau khi tạo, thêm CORS cho `PUT`/`POST`, cho phép các header cần thiết và origin của frontend.
6. Thêm lifecycle transition sang Standard-IA sau 90 ngày, Glacier Flexible Retrieval sau 365 ngày và xóa noncurrent version theo chính sách của lab.

![Thiết lập ban đầu cho raw bucket staging](/images/5-Workshop/5.3-Storage-Database/s3_raw_setup.png)

### Processed bucket

1. Tạo bucket thứ hai với ACL disabled, Block all public access và SSE-S3.
2. Không bật versioning để khớp CDK hiện tại.
3. Để S3 Bucket Key ở **Disable**.
4. Sau khi tạo, thêm CORS cho `GET`/`HEAD` và lifecycle rule hủy multipart upload chưa hoàn thành sau 7 ngày.

![Versioning và mã hóa của processed bucket](/images/5-Workshop/5.3-Storage-Database/s3_processed_setup.png)

> Cả hai bucket đều private. Frontend sử dụng presigned URL; không bật static website hosting hoặc public-read policy.

## 2. Amazon DynamoDB

Tạo ba bảng ở chế độ **On-demand**, bật mã hóa mặc định và Point-in-time recovery:

| Bảng minh họa | Partition key | Sort key | Chức năng |
|---|---|---|---|
| `SmartImage-Images-staging` | `PK` (String) | `SK` (String) | Metadata ảnh và access pattern liên quan |
| `SmartImage-UserQuotas-staging` | `PK` (String) | `SK` (String) | Hạn mức tải lên/lưu trữ |
| `SmartImage-UserProfiles-staging` | `PK` (String) | `SK` (String) | Hồ sơ người dùng |

![Khai báo khóa chính cho bảng Images](/images/5-Workshop/5.3-Storage-Database/dynamodb_table_setup.png)

### Stream của bảng Images

1. Mở bảng `Images` và chọn **Exports and streams**.
2. Bật DynamoDB Stream với view type **New and old images**.
3. Chưa tạo Lambda trigger tại đây; trigger được minh họa trong phần Backend.

### Global Secondary Indexes

Tạo hai GSI trên bảng `Images`:

- `GSI1-TagIndex-v2`: `GSI1PK` và `GSI1SK` (String), projection `INCLUDE` gồm `imageId`, `userId`, `thumbnailKey`, `originalFilename`, `createdAt`, `moderationStatus`, `imagePK`, `imageSK`.
- `GSI2-ModerationIndex`: `GSI2PK` và `GSI2SK` (String), projection `INCLUDE` gồm `imageId`, `userId`, `originalKey`, `moderationLabels`, `thumbnailKey`, `createdAt`.

![Khai báo key schema cho GSI1](/images/5-Workshop/5.3-Storage-Database/dynamodb_gsi_setup.png)

> Ảnh chỉ minh họa key schema của GSI1. Có thể chọn projection `ALL` cho bài Console ngắn, nhưng đây là phương án tùy chọn và khác với CDK.
