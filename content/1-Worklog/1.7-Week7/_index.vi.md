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
| 3 - 4 | Tạo Amazon SNS topic, đăng ký email, xác nhận subscription từ hộp thư và liên kết topic với CloudWatch Alarm. | 26/05/2026 | 27/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Tìm hiểu CloudWatch Logs. Cài đặt và cấu hình CloudWatch Agent trên máy chủ EC2 để đẩy logs hệ điều hành và ứng dụng lên Cloud. | 28/05/2026 | 28/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành: Xem metric `EstimatedCharges` tại `us-east-1`, tạo Billing Alarm và thiết kế CloudWatch Dashboard hiển thị metric của EC2 và ALB. | 29/05/2026 | 29/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 7:

* Tạo alarm cho CPU và status check của EC2; kiểm tra trạng thái `OK`, `INSUFFICIENT_DATA` và `ALARM` theo dữ liệu metric.
* Xác nhận SNS email subscription trước khi kiểm tra thông báo từ alarm.
* Cài CloudWatch Agent trên EC2 và kiểm tra log stream nhận log hệ điều hành/ứng dụng.
* Tạo Billing Alarm từ `EstimatedCharges` ở `us-east-1`. Alarm chỉ gửi cảnh báo, không tự động dừng tài nguyên.
