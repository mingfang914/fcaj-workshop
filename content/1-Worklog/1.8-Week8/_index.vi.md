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
| 2 - 3 | Viết một hàm Lambda cơ bản bằng Node.js 22.x. Cấu hình execution role chỉ cho phép đọc object từ bucket S3 dùng trong bài lab. | 01/06/2026 | 02/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Thiết lập S3 Event Notification trên một bucket nguồn để tự động gọi hàm Lambda xử lý mỗi khi có tệp tin tải lên. | 03/06/2026 | 03/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Tìm hiểu Amazon API Gateway: REST vs HTTP APIs, các khái niệm Resources, HTTP Methods (GET, POST), Stages và Deployments. | 04/06/2026 | 04/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 - 6 | Thực hành: Tạo một REST API trên API Gateway, cấu hình method GET tích hợp với Lambda và triển khai lên Stage `dev` để kiểm tra bằng curl. | 04/06/2026 | 05/06/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 8:

* Tạo Lambda Node.js 22.x với execution role giới hạn quyền đọc object từ bucket của bài lab.
* Cấu hình S3 Event Notification và xác nhận một lần tải object có thể tạo sự kiện xử lý; handler được thiết kế idempotent vì S3 có thể phân phối sự kiện nhiều hơn một lần.
* Tạo REST API với phương thức GET tích hợp Lambda Proxy Integration.
* Deploy stage `dev`, gọi endpoint bằng `curl` và kiểm tra status code cùng response body.
