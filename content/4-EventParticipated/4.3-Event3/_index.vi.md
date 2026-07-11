---
title: "Event 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch "FCAJ Intern Meetup 13/6"

### Mục Đích Của Sự Kiện

- Báo cáo tiến độ thực tập hàng tuần, nghiệm thu các kết quả triển khai hạ tầng đám mây và mã nguồn serverless của dự án cá nhân Smart Image Platform.
- Chia sẻ định hướng nghề nghiệp trong các lĩnh vực DevOps, Data Analytics và AI, giúp sinh viên làm quen với văn hóa làm việc và quy trình tuyển dụng tại các tập đoàn đa quốc gia (MNCs).
- Lắng nghe ý kiến đóng góp, phản biện kỹ thuật từ Mentor để định hướng tinh chỉnh và hoàn thiện cấu trúc hệ thống an toàn nhất.

### Danh Sách Nhóm Báo Cáo & Diễn Giả

- **Trương Hoàng Trọng** - DevOps Engineer tại Endava Vietnam (Chuyên đề: "*DevOps Career Pathway & Tools*")
- **Mr. Đạt Phạm** (Data Analytics Engineer) & **Mr. Cường Nguyễn** (Process Engineer) (Chuyên đề: "*Data Work & Corporate Culture at MNCs*")
- **Danh Hoàng Hiếu Nghị** - AI Engineer & AWS Community Builder (Chuyên đề: "*FCAJ to AWS Partner Journey*")
- **Trung Kiên và Minh Thọ** - Báo cáo tiến độ (Chuyên đề: "*Xây dựng luồng ứng dụng an toàn với Cognito, Lambda và S3*")

### Nội Dung Nổi Bật

#### 1. Định hướng nghề nghiệp DevOps thực tế (Trương Hoàng Trọng)
- Làm rõ sự khác biệt giữa mô tả công việc (JD) lý thuyết (CI/CD, IaC, K8s, Cloud) và công việc DevOps thực tế hàng ngày (giải quyết sự cố hạ tầng, điều phối hệ thống).
- Khuyên sinh viên tập trung vào các kiến thức cơ bản (Linux, Networking, lập trình Python/Golang, Git) trước khi học các công cụ nâng cao.
- Đúc rút kinh nghiệm: Copy câu lệnh không có nghĩa là hiểu bản chất, luôn tự hỏi "tại sao" trước khi hỏi "như thế nào", và giao tiếp hiệu quả là một phần thiết yếu của công việc.

#### 2. Kể chuyện bằng dữ liệu và văn hóa tập đoàn đa quốc gia (Mr. Đạt Phạm & Mr. Cường Nguyễn)
- Phân tích nhiệm vụ thực tế của một Data Analytics Engineer phụ thuộc lớn vào đặc thù ngành (domain) và yêu cầu phòng ban, đòi hỏi sự kết hợp giữa tư duy phản biện, kỹ năng giao tiếp và giải quyết vấn đề.
- Giải mã quy trình tuyển dụng chuẩn tại các MNCs: Sàng lọc hồ sơ bằng ATS, phỏng vấn nhanh bằng tiếng Anh, kiểm tra năng lực kỹ thuật và phỏng vấn hành vi.
- Chia sẻ triết lý phát triển bản thân và định nghĩa văn hóa doanh nghiệp chính là cách nghĩ, cách sống và cách làm việc của doanh nghiệp đó.

#### 3. Hành trình từ học viên FCAJ đến đối tác AWS Partner (Danh Hoàng Hiếu Nghị)
- Chia sẻ chặng đường phát triển cá nhân: Xuất phát điểm từ thành viên chương trình First Cloud Journey, trở thành Leader của AWS Student Builder Group và đạt danh hiệu AWS Community Builder.
- Chia sẻ định hướng nghề nghiệp, cách viết lịch sử bản thân và chuẩn bị năng lực để chuyển dịch từ vai trò học viên sang kỹ sư tại các đối tác của AWS (AWS Partners).

#### 4. Báo cáo kỹ thuật: Tích hợp luồng bảo mật Cognito, Lambda và S3 (Trung Kiên và Minh Thọ)
- Báo cáo tiến độ tích hợp Cognito User Pool làm Authorizer để kiểm soát và xác thực quyền truy cập API cho giao diện React SPA.
- Trình bày kiến trúc tải tệp tin an toàn: Thay vì cấp quyền đọc ghi công khai (public) cho S3, hệ thống sử dụng AWS Lambda để sinh các liên kết có thời hạn (S3 Presigned URLs) cho các tác vụ upload và view ảnh.
- Giải quyết bài toán phân quyền chặt chẽ thông qua việc gán IAM Roles và Policies tối giản cho Lambda xử lý.

### Những Gì Học Được

- **Tư duy hệ thống của DevOps:** Hiểu rõ rằng công cụ luôn thay đổi nhưng các kiến thức nền tảng (Linux, Networking) và tư duy hệ thống mới là chìa khóa giải quyết vấn đề.
- **Tác phong tuyển dụng MNC:** Nắm bắt được quy trình sàng lọc và cách thức ứng tuyển chuẩn mực tại các tập đoàn lớn để chuẩn bị hồ sơ và năng lực tiếng Anh phù hợp.
- **Kiến trúc Serverless Bảo mật:** Hiểu sâu về cách triển khai luồng xác thực Cognito kết hợp S3 Presigned URL để đảm bảo an toàn dữ liệu hình ảnh.

### Ứng Dụng Vào Công Việc

- Áp dụng giải pháp **Cognito Authorizer** vào cấu hình REST API Gateway để bảo vệ các endpoints nhạy cảm của dự án Smart Image Platform.
- Triển khai thành công **S3 Presigned URLs** tại Lambda backend cho luồng tải ảnh của người dùng, đóng hoàn toàn quyền truy cập public của S3 bucket.
- Rà soát lại toàn bộ IAM Roles của các hàm Lambda để loại bỏ các quyền dư thừa (`*`), tuân thủ quy tắc Least Privilege.

### Trải nghiệm trong event

- Buổi họp báo cáo mang tính trao đổi cởi mở nhưng đòi hỏi cao về mặt kỹ thuật, giúp người thực hiện nhận diện rõ các lỗi kiến trúc sớm.
- Các bài chia sẻ định hướng nghề nghiệp từ các anh đi trước (DevOps, Data, AI Partner) tiếp thêm động lực học tập và định hướng nghề nghiệp rõ ràng cho giai đoạn sắp tới.

#### Thư mục hình ảnh sự kiện
![Ảnh sự kiện](/images/4-EventParticipated/event3-1.png)
![Ảnh sự kiện](/images/4-EventParticipated/event3-2.png)
![Ảnh sự kiện](/images/4-EventParticipated/event3-3.png)
