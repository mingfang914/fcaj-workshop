---
title: "Event 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch "FCAJ Community Day 23/5"

### Mục Đích Của Sự Kiện

- Cung cấp các kiến thức chuyên sâu và trải nghiệm thực tế về tối ưu hóa hạ tầng đám mây AWS, triển khai trí tuệ nhân tạo (AI/LLM) và các giải pháp bảo mật tại biên.
- Tạo không gian kết nối và trao đổi kinh nghiệm công nghệ giữa các chuyên gia thực chiến từ các doanh nghiệp công nghệ lớn và cộng đồng học tập First Cloud AI Journey.
- Định hướng tư duy phát triển hệ thống AI có khả năng mở rộng, bảo mật cấp doanh nghiệp và tối ưu hóa chi phí vận hành.

### Danh Sách Diễn Giả

- **Tinh Truong** - Platform Engineer tại GoTyme Bank (Chuyên đề: "*Context Is Everything*")
- **Phạm Ngọc Hải Anh** - AWS Community Builder tại G-AsiaPacific Vietnam (Chuyên đề: "*Friendly AI Assistant w/ Amazon Q*")
- **Nguyễn Tuấn Thịnh** - DevOps Engineer (Chuyên đề: "*From Edge To Origin: CloudFront as Your Foundation*")
- **Team VIB** - Diễn giả từ LotusHacks 2026 (Chuyên đề: "*36 hrs with LotusHacks – Building UTMorpho from Idea to Reality*")
- **Đức Đào** - Solution Architect tại Cloud Kinetics (Chuyên đề: "*Non-Determinism of 'Deterministic' LLM Settings*")
- **Vy Lâm** - Senior Business Systems Analyst tại VPBank (Chuyên đề: "*Enterprise-Grade Multi-Agent System: The Case of Startup Credit Scoring*")

### Nội Dung Nổi Bật

#### 1. Vai trò của Context trong tương tác AI (Tinh Truong)
- Phân tích nguyên nhân cốt lõi khiến kết quả phản hồi của AI kém chất lượng thường xuất phát từ việc cung gian ngữ cảnh yếu (poor context) chứ không phải do năng lực mô hình.
- Chỉ ra 3 sai lầm phổ biến khi tương tác với AI: Sao chép quá nhiều tài liệu không chọn lọc gây loãng thông tin, lặp lại những kiến thức hiển nhiên đã được huấn luyện sẵn, và đặt câu hỏi không đi kèm ràng buộc cụ thể.
- Đề xuất khung chuẩn bị ngữ cảnh gồm 4 yếu tố: Mục tiêu (Goal), Thông tin liên quan (Relevant info), Ràng buộc kỹ thuật (Constraints) và Tiêu chí đánh giá thành công (Success criteria). Hướng tới xây dựng bộ não thứ hai (Second AI Brain) bằng sự kết hợp giữa Context và Memory.

#### 2. Trợ lý AI thông minh Amazon Q (Phạm Ngọc Hải Anh)
- Demo cách thức Amazon Q giải quyết bài toán thu thập và phân tích dữ liệu phân tán từ nhiều nguồn (World knowledge, Company data, User files) thông qua hơn 40 kết nối dữ liệu của Bedrock.
- Trình bày kịch bản ứng dụng Amazon Q làm trợ lý quản lý dự án (PM assistant): Tự động tạo biên bản họp (MoM), gửi email thông báo cho các bên liên quan và tự động lên lịch cuộc họp tiếp theo.

#### 3. Phân phối và bảo mật tầng biên với Amazon CloudFront (Nguyễn Tuấn Thịnh)
- Phân tích chi tiết kiến trúc mạng lưới toàn cầu của CloudFront (Edge Locations và Regional Edge Caches), các giải pháp phòng chống tấn công DDoS volumetric nhờ AWS Shield và AWS WAF.
- Giải pháp tối ưu hóa chi phí nhờ cơ chế miễn phí Data Transfer từ AWS origins sang CloudFront, giảm tải CPU cho máy chủ gốc EC2 (từ 5% xuống 1%) bằng cách chuyển giao xử lý TLS handshake và nén dữ liệu (gzip/brotli 82% size reduction).
- Giải pháp ẩn giấu máy chủ gốc (Origin Cloaking) sử dụng Origin Access Control (OAC) cho S3/Lambda và VPC Origin cho Application Load Balancer (ALB).

#### 4. Trải nghiệm Hackathon thực chiến (Team VIB)
- Chia sẻ hành trình 36 giờ thiết kế và lập trình sản phẩm UTMorpho tại LotusHacks 2026 dưới áp lực thời gian lớn.
- Nêu bật các thách thức kỹ thuật như lỗi sinh dữ liệu quá mức của AI (AI overgeneration), giới hạn token của mô hình và phương pháp tối giản hóa kiến trúc để bàn giao sản phẩm khả thi tối thiểu (MVP).

#### 5. Tính phi định tính của cài đặt định tính trong LLM (Đức Đào)
- Giải thích cơ chế lựa chọn token tiếp theo của LLM dựa trên phân phối xác suất logit qua hàm Softmax và vai trò điều chỉnh của Temperature, Top-P, Top-K.
- Làm sáng tỏ lý do vì sao cài đặt `temperature=0` vẫn tạo ra kết quả không đồng nhất trong thực tế: Do tính chất không kết hợp của toán học dấu phẩy động (floating-point arithmetic) trên GPU và cơ chế gộp các yêu cầu API từ nhiều người dùng thành một lô (inference batching).
- Đề xuất phương pháp chạy prompt nhiều lần kết hợp bỏ phiếu số đông (majority voting) và thiết lập sweet spot ở mức `temperature=0.1` để đảm bảo tính ổn định tối đa cho hệ thống production.

#### 6. Hệ thống Multi-Agent đánh giá điểm tín dụng (Vy Lâm)
- Phân tích sự bất tương đồng giữa dữ liệu tài chính của ngân hàng truyền thống và dữ liệu đa chiều của các công ty khởi nghiệp (burn rate, unit economics, tam, team capability).
- Chỉ ra hạn chế của kiến trúc Single Agent (giới hạn ngữ cảnh, phân tán chuyên môn, thiếu cơ chế kiểm soát chéo) và đề xuất kiến trúc Ủy ban tín dụng ảo (Virtual Credit Committee) phối hợp giữa nhiều tác nhân chuyên biệt (Manager, Financial Analyst, Market Analyst).
- Xây dựng mô hình triển khai thực tế từ local app sử dụng CrewAI đóng gói Container Docker đưa lên Amazon ECR và chạy trên Amazon Bedrock Agent.

### Những Gì Học Được

- **Tối ưu hóa thiết kế Context:** Hiểu được tầm quan trọng của việc kỹ thuật hóa ngữ cảnh chất lượng thay vì gia tăng số lượng tài liệu đầu vào cho LLM.
- **Bảo mật và Hiệu năng Biên:** Nắm vững cơ chế hoạt động của CloudFront OAC và cách cấu hình để bảo vệ tối đa cho tài nguyên lưu trữ S3.
- **Bản chất hoạt động của LLM:** Nhận thức được nguyên nhân gây ra tính bất định của LLM trên GPU để có các chiến lược giảm thiểu rủi ro phù hợp khi triển khai dự án thực tế.
- **Tư duy Multi-Agent:** Tiếp cận phương pháp thiết kế các tác nhân AI làm việc cộng tác và các tiêu chuẩn bảo mật, quản trị dữ liệu (PII, Encryption, Secrets management) cấp doanh nghiệp.

### Ứng Dụng Vào Công Việc

- Áp dụng giải pháp **CloudFront kết hợp OAC** để tối ưu hóa hiệu năng tải ảnh nén và bảo vệ S3 bucket của dự án Smart Image Platform.
- Sử dụng cấu hình **Temperature = 0.1** cho các API gọi mô hình Claude trên Bedrock để kiểm soát tính ổn định của đầu ra dữ liệu.
- Thiết lập khung tài liệu dự án theo mô hình **Second Brain** để quản lý tiến độ thực tập.

### Trải nghiệm trong event

- Sự kiện mang lại những góc nhìn công nghệ rất thực tiễn, các bài chia sẻ đi kèm demo trực quan giúp người học nhanh chóng tiếp thu.
- Tạo cơ hội trao đổi trực tiếp với các chuyên gia Platform, DevOps và AI thực chiến tại các doanh nghiệp lớn.

#### Thư mục hình ảnh sự kiện
- Liên kết thư mục ảnh: https://drive.google.com/drive/folders/1KF14Za3sMxDnap0HFEL-TcsTdsuGpovr
