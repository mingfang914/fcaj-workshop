---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Tìm hiểu cách thu thập số liệu giám sát hệ thống với Amazon CloudWatch.
* Thiết lập các cảnh báo lỗi tự động gửi thông báo cho quản trị viên qua email.
* Xây dựng Dashboard trực quan hóa trạng thái sức khỏe tài nguyên hạ tầng.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tìm hiểu Amazon CloudWatch: các Metrics, Namespaces, Dimensions và Resolution. | 25/05/2026 | 25/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | Cấu hình CloudWatch Alarms để giám sát các ngưỡng CPU của EC2 (>80%) và trạng thái lỗi hệ thống. | 26/05/2026 | 26/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Khởi tạo các Amazon SNS Topics và đăng ký nhận mail cho địa chỉ email quản trị. Liên kết SNS với CloudWatch Alarm để tự động gửi thông báo. | 27/05/2026 | 27/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Tìm hiểu CloudWatch Logs. Cài đặt và cấu hình CloudWatch Agent trên máy chủ EC2 để đẩy logs hệ điều hành và ứng dụng lên Cloud. | 28/05/2026 | 28/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành: Tạo một Billing Alarm cảnh báo chi phí tài khoản và thiết kế một CloudWatch Dashboard hiển thị biểu đồ hiệu năng EC2 và ALB. | 29/05/2026 | 29/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 7:

* Giám sát hiệu năng hạ tầng bằng CloudWatch Metrics và thiết lập các CloudWatch Alarms cảnh báo quá tải CPU (>80%).
* Khởi tạo SNS Topic và đăng ký nhận mail để tự động nhận tin cảnh báo từ Alarms.
* Cài đặt CloudWatch Agent trên EC2 để thu thập log hệ điều hành và logs ứng dụng tập trung.
* Thiết lập Billing Alarm cảnh báo chi phí và xây dựng CloudWatch Dashboard trực quan hóa hiệu năng.
