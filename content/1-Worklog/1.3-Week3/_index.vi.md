---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Tìm hiểu cách sử dụng dịch vụ lưu trữ đối tượng Amazon S3 và tối ưu hóa quản lý dữ liệu.
* Tìm hiểu Amazon CloudFront để phân phối tài nguyên tĩnh an toàn và cải thiện hiệu năng truy cập.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tìm hiểu Amazon S3: Các lớp lưu trữ (Standard, IA, Glacier), cách tải lên/tải xuống và quản lý siêu dữ liệu (metadata) của đối tượng. | 27/04/2026 | 27/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 - 4 | Cấu hình tính năng S3 Bucket Versioning để lưu trữ các phiên bản tệp. Thiết lập quy tắc vòng đời (Lifecycle Rules) tự động chuyển tệp cũ sang Glacier và cấu hình nhân bản chéo vùng (CRR). | 28/04/2026 | 29/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Cấu hình tính năng S3 static website hosting để chạy trang web tĩnh. Thiết lập bucket policy cho phép đọc công khai (public read access). | 30/04/2026 | 30/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 - 6 | Tìm hiểu Amazon CloudFront CDN: các Edge locations, cơ chế lưu bộ nhớ đệm (caching), quản lý TTL và bảo mật HTTPS sử dụng AWS Certificate Manager (ACM). | 30/04/2026 | 01/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành riêng mô hình CloudFront với S3 REST endpoint: giữ bucket ở trạng thái private, bật Block Public Access và cấu hình Origin Access Control (OAC) để CloudFront đọc object. | 01/05/2026 | 01/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 3:

* Thực hiện tải object, quản lý metadata, bật Versioning và kiểm tra Lifecycle Rules trên bucket dùng cho bài lab.
* Cấu hình Cross-Region Replication (CRR) giữa hai bucket để quan sát quá trình sao chép object sang Region đích.
* Phân biệt hai phương án: S3 website endpoint cần public read, còn CloudFront OAC sử dụng private S3 REST origin.
* Trong bài thực hành OAC, giữ bucket private và truy cập nội dung thông qua CloudFront. CloudFront chưa được bật trong dự án Smart Image; dự án hiện trả presigned URL của S3.
