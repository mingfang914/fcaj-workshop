---
title: "Worklog Tuần 10"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Thiết lập hệ thống đăng ký và xác thực người dùng sử dụng Amazon Cognito.
* Triển khai mô hình phân quyền người dùng RBAC dựa trên nhóm.
* Khai báo execution role và quyền tối thiểu cho backend bằng AWS CDK.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Khai báo Cognito User Pool và SPA app client không có client secret trong CDK; kiểm tra tài nguyên sau khi deploy trên Cognito Console. | 15/06/2026 | 15/06/2026 | `infrastructure/lib/stacks/auth-stack.ts` |
| 2 - 3 | Tạo hai Cognito group `admin` và `user`; xác định claim `cognito:groups` là nguồn phân quyền của frontend và backend. | 15/06/2026 | 16/06/2026 | Mã nguồn Auth stack và frontend auth service |
| 4 - 5 | Liệt kê các thao tác S3, DynamoDB, Cognito và Rekognition mà từng Lambda sử dụng; đối chiếu với quyền được cấp trong CDK. | 17/06/2026 | 18/06/2026 | `infrastructure/lib/stacks/api-stack.ts` |
| 5 | Xác nhận quyền của `ApiHandler`, `ImageProcessor` và `AIAnalyzer` bao phủ các thao tác batch/query và xóa object mà mã nguồn gọi; điều chỉnh CDK nếu phát hiện chênh lệch. | 18/06/2026 | 18/06/2026 | Backend handlers và API stack |
| 6 | Deploy stack, kiểm tra execution roles và inline policies do CloudFormation tạo trong IAM Console. | 19/06/2026 | 19/06/2026 | AWS CDK/CloudFormation outputs |

### Kết quả đạt được tuần 10:

* Tạo Cognito User Pool và SPA app client không có client secret. MFA TOTP được cấu hình theo tùy chọn của môi trường.
* Dùng claim `cognito:groups` để xác định quyền `admin`; người dùng không thuộc nhóm `admin` được frontend xử lý như người dùng thông thường.
* Thuộc tính `custom:role` còn tồn tại trong cấu hình nhưng không được dùng làm nguồn phân quyền chính. Chưa có Post Confirmation trigger tự động thêm người đăng ký mới vào group `user`.
* Tạo execution roles và quyền truy cập bằng CDK thay vì duy trì ba managed policy thủ công. Sau khi deploy, kiểm tra resource ARN và các action được cấp trong IAM Console.
