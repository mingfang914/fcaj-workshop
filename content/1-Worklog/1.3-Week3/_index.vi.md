---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

* Tìm hiểu cách sử dụng dịch vụ lưu trữ đối tượng Amazon S3 và tối ưu hóa quản lý dữ liệu.
* Tìm hiểu mạng lưới phân phối nội dung CDN (Amazon CloudFront) để phân phối tài nguyên tĩnh an sau, hiệu năng cao.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | Tìm hiểu Amazon S3: Các lớp lưu trữ (Standard, IA, Glacier), cách tải lên/tải xuống và quản lý siêu dữ liệu (metadata) của đối tượng. | 27/04/2026 | 27/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 3 - 4 | Cấu hình tính năng S3 Bucket Versioning để lưu trữ các phiên bản tệp. Thiết lập quy tắc vòng đời (Lifecycle Rules) tự động chuyển tệp cũ sang Glacier và cấu hình nhân bản chéo vùng (CRR). | 28/04/2026 | 29/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 | Cấu hình tính năng S3 static website hosting để chạy trang web tĩnh. Thiết lập bucket policy cho phép đọc công khai (public read access). | 30/04/2026 | 30/04/2026 | https://cloudjourney.awsstudygroup.com/ |
| 5 - 6 | Tìm hiểu Amazon CloudFront CDN: các Edge locations, cơ chế lưu bộ nhớ đệm (caching), quản lý TTL và bảo mật HTTPS sử dụng AWS Certificate Manager (ACM). | 30/04/2026 | 01/05/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Thực hành: Hosting một website tĩnh lên S3, tích hợp với CloudFront CDN sử dụng OAC (Origin Access Control) để chặn các truy cập trực tiếp từ Internet vào S3. | 01/05/2026 | 01/05/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 3:

* Nắm vững cách quản lý đối tượng trên S3, thiết lập bucket policies, cấu hình Versioning để bảo vệ dữ liệu và Lifecycle Rules để tối ưu chi phí lưu trữ.
* Triển khai S3 Cross-Region Replication (CRR) giúp nhân bản dữ liệu tự động giữa các vùng địa lý khác nhau.
* Cấu hình hosting trang web tĩnh trên S3 và tích hợp mạng phân phối CloudFront CDN.
* Thiết lập Origin Access Control (OAC) trên CloudFront để chặn hoàn toàn truy cập trực tiếp từ ngoài vào S3, chỉ cho phép qua CDN.
