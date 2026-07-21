---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2:

* Khởi tạo tài khoản AWS và cấu hình AWS Budgets để kiểm soát chi phí.
* Kiểm tra chương trình Free Tier và credits mà tài khoản đủ điều kiện sử dụng.
* Tìm hiểu hệ thống quản lý định danh AWS IAM và thiết lập cơ chế bảo mật xác thực đa yếu tố (MFA).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tạo tài khoản AWS mới, kiểm tra quyền lợi Free Tier/credit áp dụng cho tài khoản và cấu hình AWS Budgets với ngưỡng cảnh báo chi phí. | 20/04/2026 | 20/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 - 4 | Hoàn thành 5 nhiệm vụ làm quen trên AWS Console gồm Budgets, EC2, RDS, Lambda và Bedrock; kiểm tra credit được ghi nhận theo điều kiện của chương trình. | 21/04/2026 | 22/04/2026 | Tài liệu AWS |
| 5 | Nghiên cứu lý thuyết IAM: Người dùng (Users), Nhóm (Groups), Chính sách (Policies) và Vai trò (Roles). Cài đặt và cấu hình AWS CLI trên máy tính. | 23/04/2026 | 23/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Tạo các nhóm IAM `Admins` và `Developers`; kích hoạt MFA cho root user theo yêu cầu bảo mật hiện hành của AWS và cấu hình MFA cho các IAM user dùng trong bài thực hành. | 24/04/2026 | 24/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành: Viết chính sách bảo mật IAM (custom policy) cho phép đọc ghi S3, gán vào một user thử nghiệm và kiểm tra lại quyền hạn thông qua AWS CLI. | 24/04/2026 | 24/04/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 2:

* Thiết lập AWS Budgets với ngưỡng cảnh báo 1 USD; ngưỡng này dùng để cảnh báo, không tự động dừng tài nguyên.
* Hoàn thành 5 nhiệm vụ làm quen và kiểm tra trạng thái credit trực tiếp trong Billing Console.
* Tạo IAM users, groups và policies cho bài lab; bật MFA cho root user và các IAM user được sử dụng.
* Viết custom policy bằng JSON và kiểm tra quyền S3 thông qua một AWS CLI profile thử nghiệm.
