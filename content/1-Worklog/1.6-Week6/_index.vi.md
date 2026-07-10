---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Tìm hiểu các giải pháp phân phối lưu lượng và tự động co giãn tài nguyên trên AWS.
* Triển khai Application Load Balancer để điều hướng lưu lượng.
* Cấu hình Auto Scaling Group để tự động điều chỉnh số lượng máy chủ dựa trên tải.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tìm hiểu Elastic Load Balancing (ELB): Application, Network, Gateway Load Balancers. Cấu hình Target Groups và HTTP health checks. | 18/05/2026 | 18/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 | Khởi tạo một Application Load Balancer (ALB) trong Public Subnets. Trỏ ALB tới các máy chủ web chạy trong private subnets. | 19/05/2026 | 19/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Tìm hiểu các thành phần của Auto Scaling: Launch Templates, cấu hình dung lượng Tối thiểu/Tối đa/Mong muốn, và chính sách co giãn. | 20/05/2026 | 20/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Khởi tạo một Auto Scaling Group (ASG) đứng sau ALB. Thiết lập Target Tracking Scaling Policy dựa trên chỉ số CPU sử dụng trung bình (ví dụ: 50%). | 21/05/2026 | 21/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành: Cài đặt Apache Web Server lên các EC2 instance mới. Sử dụng công cụ Apache Bench (`ab`) để kiểm thử giả lập tải cao, xác minh hoạt động scale-out và scale-in. | 22/05/2026 | 22/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 6:

* Cấu hình Target Groups, HTTP health checks và khởi tạo Application Load Balancer (ALB) trong Public Subnets.
* Thiết lập Launch Templates và cấu hình Auto Scaling Group (ASG) đứng sau ALB trên nhiều AZs.
* Cấu hình Target Tracking Scaling Policy tự động thêm/bớt instance dựa trên chỉ số CPU sử dụng trung bình.
* Chạy kiểm thử tải bằng công cụ Apache Bench (ab) để kiểm tra hoạt động co giãn tự động (scale-out/scale-in) của ASG.
