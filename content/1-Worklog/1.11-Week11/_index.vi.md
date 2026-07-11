---
title: "Worklog Tuần 11"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Triển khai mã nguồn backend lên ba hàm Lambda sử dụng tài nguyên tối ưu.
* Kết nối các sự kiện lưu trữ và dữ liệu cơ sở dữ liệu để kích hoạt tự động các tiến trình xử lý.
* Cấu hình API Gateway REST API và tích hợp Cognito Authorizer bảo mật.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Khởi tạo các hàm Lambda (`ApiHandler`, `ImageProcessor`, `AIAnalyzer`) trên Console, chọn runtime Node.js 22.x, kiến trúc ARM64 và gán các vai trò thực thi tương ứng. | 22/06/2026 | 22/06/2026 |  |
| 3 - 5 | Nén và tải lên các gói mã nguồn (bao gồm sharp cho xử lý ảnh và AWS SDK), thiết lập các biến môi trường kết nối tới các bảng DynamoDB và S3 buckets. | 23/06/2026 | 25/06/2026 |  |
| 5 | Cấu hình S3 Event Notification kích hoạt `ImageProcessor` (lọc prefix `users/`) và cấu hình DynamoDB Stream kích hoạt `AIAnalyzer` (batch size: 1). | 25/06/2026 | 25/06/2026 |  |
| 6 | Khởi tạo REST API `SmartImage-API` và cấu hình bộ Cognito Authorizer bảo mật. | 26/06/2026 | 26/06/2026 |  |
| 6 | Thực hành: Tạo resource proxy `{proxy+}` có bật CORS, thiết lập method ANY tích hợp proxy tới `ApiHandler` và deploy lên stage `dev`. | 26/06/2026 | 26/06/2026 |  |

### Kết quả đạt được tuần 11:

* Triển khai và chạy thử nghiệm thành công 3 hàm Lambda serverless sử dụng kiến trúc ARM64 giúp giảm độ trễ và tiết kiệm chi phí.
* Cấu hình S3 Event Notification kích hoạt `ImageProcessor` khi tải tệp lên raw bucket tại thư mục `users/`, tự động hóa quy trình nén ảnh bằng thư viện Sharp và lưu trữ sang processed bucket.
* Bật DynamoDB Stream tích hợp kích hoạt `AIAnalyzer` khi có thay đổi bản ghi, tự động gọi Amazon Rekognition để dán thẻ hình ảnh và kiểm duyệt nội dung nhạy cảm.
* Thiết lập thành công REST API trên API Gateway với tài nguyên proxy `{proxy+}` và phương thức `ANY` trỏ về `ApiHandler` sử dụng cơ chế Lambda Proxy Integration.
* Tích hợp bộ Cognito Authorizer (`CognitoAuth`) bảo vệ các endpoint API Gateway, chỉ cho phép các request chứa ID Token hợp lệ đi qua.
