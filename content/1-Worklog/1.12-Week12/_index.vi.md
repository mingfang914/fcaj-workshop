---
title: "Worklog Tuần 12"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Mục tiêu tuần 12:

* Triển khai và hosting ứng dụng frontend React Client trên AWS Amplify.
* Thực hiện kiểm thử tích hợp toàn bộ hệ thống từ đăng ký, đăng nhập đến tải ảnh lên và nhận diện nhãn.
* Dọn dẹp tài nguyên thử nghiệm và kiểm tra các tài nguyên được giữ lại theo removal policy.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Kết nối kho mã nguồn GitHub chứa React client với AWS Amplify Console, cấu hình tệp `amplify.yml`. | 29/06/2026 | 29/06/2026 |  |
| 2 - 3 | Thiết lập các biến môi trường client (UserPool ID, Client ID, API Invoke URL) và deploy ứng dụng. | 29/06/2026 | 30/06/2026 |  |
| 4 - 5 | Chạy thử nghiệm E2E: Đăng ký, đăng nhập, tải ảnh JPEG/PNG lên, xác minh hiển thị ảnh nén và kiểm duyệt nhãn AI. | 01/07/2026 | 02/07/2026 |  |
| 5 | Kiểm tra validation của frontend với định dạng không hỗ trợ; dùng ảnh có định dạng hợp lệ nhưng dữ liệu hỏng để kiểm tra log và trạng thái lỗi của backend. Đối chiếu hành vi thực tế với CloudWatch Alarm và DLQ. | 02/07/2026 | 02/07/2026 | Frontend uploader, Lambda handlers và Monitoring stack |
| 6 | Làm trống các bucket thuộc môi trường thử nghiệm, chạy `cdk destroy --all`, sau đó kiểm tra S3, DynamoDB, CloudWatch Logs, SQS, SNS và WAF để phát hiện tài nguyên còn lại. | 03/07/2026 | 03/07/2026 | CDK stacks và AWS Billing Console |

### Kết quả đạt được tuần 12:

* Kết nối repository với Amplify Hosting và sử dụng `amplify.yml` để build React client trong npm workspace monorepo.
* Cấu hình `VITE_API_URL`, `VITE_USER_POOL_ID` và `VITE_CLIENT_ID`; kiểm tra frontend gọi đúng API của môi trường triển khai.
* Kiểm tra luồng đăng ký, xác nhận email, đăng nhập, lấy presigned URL, tải JPEG/PNG và theo dõi trạng thái cho đến khi metadata ảnh cùng nhãn Rekognition được trả về.
* Frontend từ chối định dạng không hỗ trợ trước khi upload. Các Lambda xử lý hiện bắt lỗi mà không ném lại, vì vậy invocation có thể không làm tăng metric `Errors`, không kích hoạt retry/DLQ và không tạo SNS alarm như kịch bản ban đầu. Nội dung cảnh báo chỉ được ghi nhận sau khi sửa error handling hoặc thực hiện một bài kiểm tra alarm độc lập.
* Chạy `cdk destroy --all` cho tài nguyên thử nghiệm và kiểm tra thủ công tài nguyên còn lại. Không khẳng định chi phí bằng 0 vì production resources có thể dùng `RETAIN`, còn log, dữ liệu hoặc tài nguyên ngoài stack vẫn có thể phát sinh chi phí.
