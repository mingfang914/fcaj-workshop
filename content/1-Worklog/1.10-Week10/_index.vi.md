---
title: "Worklog Tuần 10"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Thiết lập hệ thống đăng ký và xác thực người dùng sử dụng Amazon Cognito.
* Triển khai mô hình phân quyền người dùng RBAC dựa trên nhóm.
* Khởi tạo các chính sách phân quyền và vai trò IAM bảo mật tối thiểu cho backend.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tạo Amazon Cognito User Pool và SPA Web Client (tắt client secret) sử dụng trình hướng dẫn nhanh. | 15/06/2026 | 15/06/2026 |  |
| 3 | Cấu hình thuộc tính tùy biến `custom:role` và tạo các nhóm quản trị (`admin`, `user`) trong Cognito. | 16/06/2026 | 16/06/2026 |  |
| 4 | Phân tích quyền hạn của các hàm backend đối với S3, DynamoDB, Cognito. Thiết kế các khối quyền JSON tương ứng. | 17/06/2026 | 17/06/2026 |  |
| 5 | Khởi tạo các IAM Policies tùy chỉnh trước (`SmartImage-ApiHandler-Policy`, `SmartImage-ImageProcessor-Policy`, và `SmartImage-AIAnalyzer-Policy`) trong phần Access Management. | 18/06/2026 | 18/06/2026 |  |
| 6 | Thực hành: Tạo các Lambda service roles trong mục IAM Roles, liên kết các chính sách bảo mật đã tạo ở Bước 5 để hoàn thành các execution roles. | 19/06/2026 | 19/06/2026 |  |

### Kết quả đạt được tuần 10:

* Khởi tạo Amazon Cognito User Pool đi kèm SPA Web Client tắt client secret để cho phép ứng dụng SPA React giao tiếp trực tiếp.
* Tích hợp thuộc tính tùy biến `custom:role` và thiết lập 2 nhóm người dùng (`admin` độ ưu tiên 0, `user` độ ưu tiên 10) hỗ trợ phân quyền vai trò.
* Thiết lập các chính sách IAM tùy chỉnh trước trong mục Access Management để tránh lỗi khi gán vai trò (`SmartImage-ApiHandler-Policy`, `SmartImage-ImageProcessor-Policy`, `SmartImage-AIAnalyzer-Policy`).
* Tạo lập 3 vai trò thực thi (execution roles) cho Lambda bảo đảm quyền hạn tối thiểu đối với S3 buckets, DynamoDB tables, và Rekognition.
