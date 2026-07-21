---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Khởi động dự án chính thức Smart Image Platform.
* Xác định access pattern và thiết kế dữ liệu DynamoDB cho metadata ảnh, quota và hồ sơ người dùng.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Phân tích yêu cầu của Smart Image Platform, xác định các luồng upload, xử lý, phân tích và truy vấn ảnh; cập nhật sơ đồ kiến trúc serverless. | 08/06/2026 | 08/06/2026 | Mã nguồn và tài liệu dự án `AWS-Project` |
| 3 - 4 | Thiết kế composite key cho bảng `Images` (`PK = USER#<id>`, `SK = IMG#<timestamp>#<ulid>`) dựa trên các access pattern của ứng dụng. | 09/06/2026 | 10/06/2026 | `infrastructure/lib/stacks/database-stack.ts` |
| 4 | Khai báo raw bucket và processed bucket trong CDK; bật Block Public Access và mã hóa SSE-S3 cho cả hai bucket. | 10/06/2026 | 10/06/2026 | `infrastructure/lib/stacks/storage-stack.ts` |
| 5 | Khai báo ba bảng DynamoDB `Images`, `UserQuotas` và `UserProfiles` ở chế độ On-demand; tên tài nguyên có hậu tố môi trường. | 11/06/2026 | 11/06/2026 | `infrastructure/lib/stacks/database-stack.ts` |
| 5 - 6 | Cấu hình `GSI1-TagIndex-v2` cho truy vấn theo tag và `GSI2-ModerationIndex` cho hàng đợi kiểm duyệt. | 11/06/2026 | 12/06/2026 | `infrastructure/lib/stacks/database-stack.ts` |

### Kết quả đạt được tuần 9:

* Tạo hai S3 bucket private bằng CDK. Client tải ảnh lên bằng presigned URL; ứng dụng hiện trả presigned URL của S3 để đọc ảnh vì CloudFront chưa được bật.
* Sử dụng ba bảng DynamoDB: `Images`, `UserQuotas` và `UserProfiles`. Chỉ bảng `Images` áp dụng single-table pattern cho metadata ảnh và các bản ghi liên quan; quota và profile được tách theo trách nhiệm nghiệp vụ.
* Gắn hậu tố `staging` hoặc `production` vào tên bảng để tách môi trường. Tên vật lý của S3 bucket do CDK tạo có thêm chuỗi duy nhất.
* Kiểm tra hai GSI theo đúng access pattern: tìm ảnh theo tag và liệt kê ảnh cần kiểm duyệt.
