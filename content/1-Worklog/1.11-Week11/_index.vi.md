---
title: "Worklog Tuần 11"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Triển khai ba Lambda nghiệp vụ bằng AWS CDK và kiểm tra cấu hình runtime, kiến trúc, bộ nhớ và timeout.
* Kết nối các sự kiện lưu trữ và dữ liệu cơ sở dữ liệu để kích hoạt tự động các tiến trình xử lý.
* Cấu hình API Gateway REST API và tích hợp Cognito Authorizer bảo mật.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Khai báo `ApiHandler`, `ImageProcessor` và `AIAnalyzer` bằng CDK trên kiến trúc ARM64; kiểm tra memory, timeout, temporary storage và execution role của từng hàm. | 22/06/2026 | 22/06/2026 | `infrastructure/lib/stacks/api-stack.ts` |
| 3 - 5 | Bundle mã TypeScript bằng esbuild qua `NodejsFunction`; đóng gói Sharp cho Linux ARM64 và cấu hình đầy đủ biến môi trường kết nối S3/DynamoDB. | 23/06/2026 | 25/06/2026 | API stack và backend handlers |
| 5 | Cấu hình S3 Event Notification gọi `ImageProcessor` với prefix `users/`; cấu hình DynamoDB Stream gọi `AIAnalyzer` với batch size 10, 3 lần retry và SQS DLQ. | 25/06/2026 | 25/06/2026 | `infrastructure/lib/stacks/api-stack.ts` |
| 6 | Khai báo REST API và Cognito User Pool Authorizer bằng CDK; kiểm tra các endpoint công khai và endpoint yêu cầu xác thực. | 26/06/2026 | 26/06/2026 | `infrastructure/lib/stacks/api-stack.ts` |
| 6 | Tạo từng resource và method `/v1/profile`, `/v1/images`, `/v1/admin/...`; cấu hình CORS và deploy theo stage của môi trường. | 26/06/2026 | 26/06/2026 | API stack và API handler router |

### Kết quả đạt được tuần 11:

* Deploy ba Lambda nghiệp vụ trên ARM64 bằng CDK. Chưa thực hiện benchmark riêng nên không đưa ra kết luận định lượng về độ trễ hoặc mức tiết kiệm chi phí.
* `ImageProcessor` nhận sự kiện từ raw bucket, sử dụng Sharp được bundle cho Linux ARM64 và ghi kết quả sang processed bucket.
* `AIAnalyzer` nhận DynamoDB Stream với batch size 10, retry tối đa 3 lần và SQS on-failure destination. Hàm cần `RAW_BUCKET_NAME` cùng các biến môi trường bảng/bucket khác.
* API Gateway sử dụng các resource và method khai báo rõ ràng, không sử dụng `{proxy+}`/`ANY`. `/v1/images/public` là endpoint công khai; các endpoint còn lại được gắn Cognito User Pool Authorizer theo cấu hình.
* CDK hiện khai báo Node.js 20.x trong API stack. Đây là điểm cần nâng lên Node.js 22.x để phù hợp runtime được hỗ trợ trong năm 2026; worklog không ghi nhận Node.js 22.x là trạng thái đã triển khai khi mã nguồn chưa được cập nhật.
