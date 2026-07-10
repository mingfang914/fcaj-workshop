---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Mục tiêu tuần 9:

* Khởi động dự án chính thức Smart Image Platform.
* Thiết kế cấu trúc bảng đơn DynamoDB Single-Table Design tối ưu hóa hiệu năng và chi phí đọc ghi.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Nghiên cứu tài liệu nghiệp vụ dự án Smart Image Platform và vẽ sơ đồ kiến trúc hệ thống Serverless. | 08/06/2026 | 08/06/2026 |  |
| 3 | Thiết kế cơ sở dữ liệu Single-Table cho DynamoDB (Composite keys: PK = USER#<id>, SK = IMG#<timestamp>#<ulid>) để tối ưu hóa hiệu năng truy vấn. | 09/06/2026 | 09/06/2026 |  |
| 4 | Tạo các S3 buckets raw và processed trên Console, thiết lập chính sách Block All Public Access bảo mật. | 10/06/2026 | 10/06/2026 |  |
| 5 | Khởi tạo các bảng chính của hệ thống: SmartImage-Images, SmartImage-UserQuotas, và SmartImage-UserProfiles trên Console ở chế độ On-demand. | 11/06/2026 | 11/06/2026 |  |
| 6 | Thực hành: Cấu hình các chỉ mục phụ toàn cục (GSI1-TagIndex-v2, GSI2-ModerationIndex) với các thuộc tính chiếu cần thiết để giảm tải RCU. | 12/06/2026 | 12/06/2026 |  |

### Kết quả đạt được tuần 9:

* Khởi tạo thành công 2 S3 buckets (`smartimage-raw-bucket` và `smartimage-processed-bucket`) cấu hình Block All Public Access bảo mật tuyệt đối, sử dụng Presigned URL cho Client tải ảnh lên.
* Thiết kế cấu trúc bảng đơn (Single-Table Design) trong DynamoDB giúp gộp chung dữ liệu người dùng, quota và ảnh vào các bảng: `SmartImage-Images`, `SmartImage-UserQuotas`, và `SmartImage-UserProfiles`.
* Cấu hình thành công chỉ mục phụ toàn cục `GSI1-TagIndex-v2` và `GSI2-ModerationIndex` giúp truy vấn ảnh theo thẻ tag và trạng thái kiểm duyệt hình ảnh tối ưu với chế độ On-demand.
