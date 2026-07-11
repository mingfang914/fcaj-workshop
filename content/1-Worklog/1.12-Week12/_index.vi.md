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
* Dọn dẹp sạch tài nguyên sau khi hoàn thành thực tập.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Kết nối kho mã nguồn GitHub chứa React client với AWS Amplify Console, cấu hình tệp `amplify.yml`. | 29/06/2026 | 29/06/2026 |  |
| 2 - 3 | Thiết lập các biến môi trường client (UserPool ID, Client ID, API Invoke URL) và deploy ứng dụng. | 29/06/2026 | 30/06/2026 |  |
| 4 - 5 | Chạy thử nghiệm E2E: Đăng ký, đăng nhập, tải ảnh JPEG/PNG lên, xác minh hiển thị ảnh nén và kiểm duyệt nhãn AI. | 01/07/2026 | 02/07/2026 |  |
| 5 | Kiểm thử lỗi bằng cách tải tệp tin không hợp lệ, truy vết logs trong CloudWatch Logs, theo dõi biểu đồ trên Dashboard và xác nhận email cảnh báo từ SNS. | 02/07/2026 | 02/07/2026 |  |
| 6 | Thực hành: Thực hiện xóa dữ liệu trong các S3 buckets và chạy lệnh `cdk destroy --all` (hoặc dọn dẹp trên Console) để giải phóng tài nguyên. | 03/07/2026 | 03/07/2026 |  |

### Kết quả đạt được tuần 12:

* Kết nối thành công kho mã nguồn GitHub với AWS Amplify Console và cấu hình tệp `amplify.yml` hỗ trợ quá trình build tự động trong cấu trúc npm workspace monorepo.
* Triển khai thành công ứng dụng React và cấu hình đầy đủ các biến môi trường Client (`VITE_API_URL`, `VITE_USER_POOL_ID`, `VITE_CLIENT_ID`) để kết nối tới backend.
* Kiểm thử E2E thành công toàn bộ luồng: Đăng ký người dùng mới, nhận mã xác thực qua email từ Cognito, đăng nhập, tải ảnh qua S3 Presigned URL, và kiểm tra hiển thị ảnh nén cùng thẻ tag AI.
* Chạy thử nghiệm kịch bản lỗi bằng cách tải lên tệp không hỗ trợ, giám sát logs lỗi thành công trong CloudWatch Log Group `/aws/lambda/SmartImage-ImageProcessor` và nhận email cảnh báo từ SNS.
* Thực hiện quy trình dọn dẹp bằng cách làm trống S3 buckets và chạy lệnh `cdk destroy --all` để xóa bỏ hoàn toàn tài nguyên, đưa chi phí vận hành tài khoản về $0.
