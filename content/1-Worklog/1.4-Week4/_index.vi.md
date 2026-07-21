---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Tìm hiểu các thành phần cấu thành mạng riêng ảo Amazon VPC.
* Thiết kế hạ tầng mạng phân mảnh có bảo mật cao trải dài trên nhiều vùng sẵn sàng (AZs).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tìm hiểu các khái niệm VPC: Địa chỉ IP, dải CIDR, phân chia subnets. Thiết kế dải CIDR cho một VPC tùy chỉnh. | 04/05/2026 | 04/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 - 4 | Tạo các Public Subnets và Private Subnets trên 2 Availability Zones khác nhau. Thiết lập Route Tables và cấu hình Internet Gateway (IGW). | 05/05/2026 | 06/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Khởi tạo NAT Gateway trong Public Subnet, định tuyến lưu lượng outbound của Private Subnets qua NAT Gateway và ghi nhận chi phí theo giờ/lưu lượng của tài nguyên này. | 06/05/2026 | 06/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Tìm hiểu và so sánh cơ chế hoạt động của Security Groups (stateful) và Network ACLs (NACLs) (stateless). Cấu hình quy tắc tường lửa tương ứng. | 07/05/2026 | 07/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành mô hình jump host: tạo Bastion Host trong Public Subnet, kết nối SSH đến web server trong Private Subnet và kiểm tra outbound internet qua NAT. So sánh với phương án AWS Systems Manager Session Manager. | 08/05/2026 | 08/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 4:

* Thiết kế dải mạng Amazon VPC tùy chỉnh, phân chia Public/Private subnets trải trên nhiều Vùng sẵn sàng (AZs).
* Thiết lập Route Tables, gán Internet Gateway cho Public Subnet và định tuyến outbound của Private Subnet qua NAT Gateway.
* Phân biệt và cấu hình tường lửa 2 lớp: Security Groups (stateful) và Network ACLs (stateless) để kiểm soát lưu lượng.
* Kiểm tra kết nối qua Bastion Host trong phạm vi bài lab; xóa NAT Gateway và các EC2 instance sau khi hoàn thành để tránh chi phí tiếp tục phát sinh.
