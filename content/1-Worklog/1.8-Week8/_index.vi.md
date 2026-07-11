---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8:

* Tìm hiểu lý thuyết lập trình Serverless và mô hình hoạt động của AWS Lambda.
* Xây dựng luồng xử lý tự động kích hoạt bởi các sự kiện lưu trữ.
* Cấu hình REST API với API Gateway làm cổng kết nối cho backend serverless.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tìm hiểu mô hình Serverless: Ưu điểm, giới hạn tài nguyên của AWS Lambda, và mô hình tính giá pay-as-you-go. | 01/06/2026 | 01/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 2 - 3 | Viết một hàm Lambda cơ bản bằng Node.js (phiên bản 22.x hoặc 24.x). Cấu hình IAM Role gán quyền cho phép Lambda đọc tệp tin từ S3. | 01/06/2026 | 02/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Thiết lập S3 Event Notification trên một bucket nguồn để tự động gọi hàm Lambda xử lý mỗi khi có tệp tin tải lên. | 03/06/2026 | 03/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Tìm hiểu Amazon API Gateway: REST vs HTTP APIs, các khái niệm Resources, HTTP Methods (GET, POST), Stages và Deployments. | 04/06/2026 | 04/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 - 6 | Thực hành: Tạo một REST API trên API Gateway, cấu hình method GET tích hợp với Lambda và triển khai lên Stage `dev` để kiểm tra bằng curl. | 04/06/2026 | 05/06/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 8:

* Nghiên cứu lý thuyết mô hình Serverless và viết hàm Lambda cơ bản bằng Node.js.
* Thiết lập S3 Event Notification để tự động gọi hàm Lambda xử lý mỗi khi có tệp tải lên.
* Khởi tạo REST API trên API Gateway, cấu hình phương thức GET trỏ về Lambda sử dụng Lambda Proxy Integration.
* Triển khai API Gateway lên Stage `dev` và thực hiện kiểm tra bằng lệnh curl.
