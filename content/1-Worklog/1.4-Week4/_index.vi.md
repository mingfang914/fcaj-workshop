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
| 4 | Khởi tạo một NAT Gateway đặt tại Public Subnet. Thiết lập định tuyến cho Private Subnets đi ra internet qua NAT Gateway. | 06/05/2026 | 06/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Tìm hiểu và so sánh cơ chế hoạt động của Security Groups (stateful) và Network ACLs (NACLs) (stateless). Cấu hình quy tắc tường lửa tương ứng. | 07/05/2026 | 07/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành: Tạo một Bastion Host ở Public Subnet và một web server ở Private Subnet. Kết nối SSH vào máy chủ Private Subnet qua Bastion Host và kiểm tra kết nối internet qua NAT. | 08/05/2026 | 08/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 4:

* Thiết kế dải mạng Amazon VPC tùy chỉnh, phân chia Public/Private subnets trải trên nhiều Vùng sẵn sàng (AZs).
* Thiết lập Route Tables, gán Internet Gateway cho Public Subnet và NAT Gateway cho Private Subnet đi ra ngoài.
* Phân biệt và cấu hình tường lửa 2 lớp: Security Groups (stateful) và Network ACLs (stateless) để kiểm soát lưu lượng.
* Triển khai Bastion Host trong Public Subnet làm jump box để SSH an toàn vào web server đặt trong Private Subnet.
