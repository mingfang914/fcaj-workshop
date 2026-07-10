---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch "FCAJ Sharing Session 9/5"

### Mục Đích Của Sự Kiện

- Chia sẻ các phương pháp học tập hiệu quả, cách ứng dụng tâm lý học hành vi để duy trì động lực tự học và phát triển kỹ năng công nghệ.
- Hướng dẫn viết prompt tối ưu, quản lý chi phí token và áp dụng các kỹ thuật lập luận AI nâng cao khi xây dựng ứng dụng.
- Định hướng tư duy làm việc chuyên nghiệp, xây dựng tinh thần trách nhiệm (integrity) và cách thức thiết lập mục tiêu dài hạn cho sinh viên chuẩn bị tốt nghiệp.
- Giới thiệu phương pháp phát triển phần mềm theo tài liệu thiết kế (Document-Driven) thông qua mô hình BMX và hệ thống Multi-Agent.

### Danh Sách Diễn Giả

- **Long Huỳnh** - Cloud Engineer, Program Administrator tại FCAJ
- **Thịnh Nguyễn** - Cloud Engineer | DevOps Engineer tại FCAJ
- **Khang Nguyen** - Solution Architect tại CloudKinetics
- **Thao Nguyen Phuong** - Application Cloud Dev tại VIB

### Nội Dung Nổi Bật

#### 1. Phương pháp học tập "Brain Hack" kích thích Dopamine
- Phân tích cơ chế trì hoãn của não bộ: Người học có xu hướng ưu tiên các phần thưởng ngắn hạn (mạng xã hội, game) thay vì học tập (nhận phần thưởng dài hạn).
- Ứng dụng Dopamine: Thiết lập hệ thống phần thưởng ngẫu nhiên sau mỗi 10-15 phút tập trung cao độ để tạo cảm giác hưng phấn cho não bộ.
- Áp dụng tâm lý học hành vi:
  - *Loss Aversion:* Tận dụng tâm lý sợ mất chuỗi (streak) để duy trì kỷ luật học tập hàng ngày.
  - *2-Minute Rule & Chunking:* Chia nhỏ các khối lượng kiến thức khổng lồ thành các nhiệm vụ cực kỳ nhỏ (đọc 1 trang tài liệu, tạo 1 tài khoản đám mây) để dễ dàng bắt đầu.

#### 2. Kỹ thuật Prompt Engineering và tối ưu hóa tài nguyên AI
- Giới thiệu cấu trúc prompt chuẩn gồm 7 thành phần: Vai trò (Role), Chỉ thị (Instruction), Ngữ cảnh (Context), Dữ liệu đầu vào (Input Data), Định dạng đầu ra (Output Format), Ví dụ minh họa (Examples) và Ràng buộc (Constraints).
- Tối ưu hóa chi phí: Phân tích cơ chế mã hóa token và lưu ý ngôn ngữ tiếng Việt tiêu tốn lượng token gấp đôi so với tiếng Anh, từ đó cần tối ưu hóa cấu trúc prompt để giảm chi phí API.
- Các kỹ thuật lập luận AI nâng cao: So sánh Chain-of-Thought (chuỗi tư duy), Self-Consistency (tính nhất quán) và Tree-of-Thought (cây tư duy).

#### 3. Tư duy làm việc và định vị bản thân trong kỷ nguyên AI
- AI là công cụ khuếch đại: Phân tích thực tế tuyển dụng khi hầu hết bài thi của ứng viên đều có sự can thiệp của AI nhưng thiếu sự thấu hiểu bản chất. Nhấn mạnh việc không được "outsource" sự thấu hiểu cho AI.
- Tầm quan trọng của câu hỏi "Tại sao" (Why): Chuyển dịch từ việc chỉ quan tâm làm thế nào để hoàn thành tác vụ (What) sang việc hiểu rõ lý do lựa chọn giải pháp phù hợp với ngữ cảnh doanh nghiệp.
- Hệ thống lợi ích dài hạn: Định hướng sinh viên phân bổ mục tiêu dựa trên 4 cột trụ: Kinh nghiệm (Experience), Mạng lưới kết nối (Network), Kiến thức (Knowledge) và Sự phát triển (Growth).
- Đề cao tính chính trực (Integrity): Proactively xử lý các trường hợp biên (edge cases) và sẵn sàng tích lũy kinh nghiệm từ các sai sót công việc.

#### 4. Quy trình phát triển phần mềm bằng phương pháp BMX
- Giải quyết vấn đề "Junk Code" (mã nguồn rác) do lạm dụng cửa sổ chat AI thông thường, dẫn đến việc quá tải và tràn Context Window.
- Triết lý Document-Driven: Quản lý dự án chặt chẽ thông qua hệ thống tài liệu thiết kế (VOD/Architect file). Khi tài liệu được thiết kế chuẩn xác, AI sẽ tự động sinh mã nguồn có tỷ lệ lỗi cực thấp.
- Phân rã vai trò trong hệ thống Multi-Agent:
  - *PM & Architect Agent:* Thiết kế kiến trúc và viết tài liệu kỹ thuật.
  - *PO & Scrum Master Agent:* Chia nhỏ tài liệu thành các User Stories cô lập.
  - *Developer Agent:* Viết mã nguồn cho từng tác vụ riêng biệt.
  - *Review Agent (QA/QC):* Chạy thử nghiệm tự động để kiểm soát lỗi trước khi tích hợp.

### Những Gì Học Được

- **Tự quản trị bản thân:** Kỹ năng phân rã mục tiêu học tập và ứng dụng Dopamine để duy trì nhịp độ tự nghiên cứu.
- **Kỹ nghệ Prompt:** Cách xây dựng prompt cấu trúc chặt chẽ và tư duy tối ưu hóa chi phí vận hành hệ thống AI.
- **Tư duy thiết kế hệ thống:** Tầm quan trọng của việc viết tài liệu kỹ thuật hoàn chỉnh trước khi bắt đầu lập trình.
- **Thái độ nghề nghiệp:** Nhận thức về sự chính trực tại nơi làm việc và kỹ năng cộng tác nhóm hiệu quả.

### Ứng Dụng Vào Công Việc

- Áp dụng cấu trúc prompt 7 thành phần để nâng cao chất lượng code sinh ra bởi AI khi xây dựng các hàm Lambda cho dự án Smart Image Platform.
- Triển khai phương pháp tài liệu hóa thiết kế (Document-Driven) cho các phân hệ backend của dự án trước khi thực hiện viết code.
- Áp dụng quy tắc 2 phút để xử lý ngay các tác vụ cấu hình hạ tầng đám mây nhỏ hàng ngày.

### Trải nghiệm trong event

- Buổi chia sẻ rất gần gũi nhưng đem lại lượng kiến thức lớn, từ tư duy lập trình với AI đến cách thức chuẩn bị hồ sơ năng lực và kỹ năng mềm cho môi trường doanh nghiệp.
- Nhận được nhiều lời khuyên sâu sắc từ các diễn giả có kinh nghiệm làm việc tại VIB và CloudKinetics.

#### Thư mục hình ảnh sự kiện
- Liên kết video sự kiện: https://www.youtube.com/watch?v=4hEntEh-nm4
