---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

* Nghiên cứu cơ chế vận hành của cơ sở dữ liệu quan hệ (RDS) và phi quan hệ (DynamoDB) trên AWS.
* Triển khai cơ sở dữ liệu quan hệ trong private subnets với độ sẵn sàng cao.
* Tìm hiểu cách thao tác và cấu hình chỉ mục trong NoSQL DynamoDB.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tìm hiểu Amazon RDS: Các công cụ DB, mô hình hoạt động Multi-AZ để tăng độ sẵn sàng, và cơ chế Read Replicas. | 11/05/2026 | 11/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | Khởi tạo một DB instance RDS MySQL trong Private Subnet. Thiết lập DB Subnet Groups và cấu hình Security Groups giới hạn truy cập từ EC2. | 12/05/2026 | 12/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Kết nối và chạy truy vấn thử nghiệm trên RDS MySQL từ máy chủ EC2. Thực hiện kiểm thử tính năng Reboot with failover để xem cơ chế chuyển đổi máy chủ của Multi-AZ. | 13/05/2026 | 13/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Tìm hiểu Amazon DynamoDB NoSQL: Khóa phân vùng (Partition Key - PK), Khóa sắp xếp (Sort Key - SK), chỉ mục phụ toàn cục (GSI) và chỉ mục phụ cục bộ (LSI). | 14/05/2026 | 14/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành: Tạo một bảng DynamoDB trên Console và thực hành chạy các lệnh CRUD cơ bản bằng cú pháp PartiQL và AWS CLI. | 15/05/2026 | 15/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 5:

* Khởi tạo máy chủ RDS MySQL trong Private Subnet sử dụng DB Subnet Groups để cô lập dữ liệu.
* Thiết lập cơ chế độ sẵn sàng cao RDS Multi-AZ và kiểm thử tình huống tự động chuyển đổi máy chủ (failover) từ EC2.
* Tìm hiểu cấu trúc dữ liệu NoSQL DynamoDB, phân biệt Partition Key và Sort Key để tối ưu hóa truy vấn.
* Thực hành chạy các thao tác CRUD cơ bản trên bảng DynamoDB thông qua PartiQL và AWS CLI.
