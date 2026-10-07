# Kiến Trúc Hệ Thống Multi-Agent Sản Xuất Video Sử Thi Thực Chiến

## 1. Bản Chất Dự Án & Định Hướng Nội Dung
- **Mục tiêu:** Tự động hóa sản xuất Video Ngắn (Shorts) dạng hoạt hình động, mang phong cách sử thi, hoành tráng đầy chiều sâu về đề tài Kinh Thánh nhằm mục đích kiếm tiền và tăng trưởng YouTube.
- **Dữ liệu Kênh hiện tại:** 722 Subscribers, 83 Giờ xem. Hệ thống phải liên tục đọc, phân tích số liệu này qua YouTube Analytics API để đưa ra các nhắc nhở chiến lược tăng trưởng, tối ưu hóa thời gian giữ chân người xem (Retention Rate).
- **Môi trường:** Python (OOP, Asyncio ngầm), giao diện NiceGUI hiển thị Dashboard local.

## 2. Quy Trình Vận Hành Và Dòng Chảy Token Của Hệ Thống

### MODULE 1: BỘ NÃO ĐIỀU PHỐI & PHÂN TÍCH THỊ TRƯỜNG (Agent 1 - Orchestrator)
- **Nhiệm vụ 1 (Cào dữ liệu đề xuất):** Sử dụng các thư viện cào mã nguồn mở (Crawl4AI/Firecrawl) kết hợp API để quét YouTube, tìm kiếm các video đang lên xu hướng có lượng quan tâm và giá trị cao.
- **Nhiệm vụ 2 (Đọc và Phân tích Đối thủ):** Tự động bóc tách dữ liệu từ các đối thủ lớn (mô phỏng cơ chế của Vibiq) để tìm ra các từ khóa, cấu trúc giữ chân người xem hiệu quả.
- **Nhiệm vụ 3 (Xử lý tài liệu Dify):** Đọc các file kịch bản/tài liệu chưa hoàn chỉnh của người dùng thông qua kết nối API với Dify. AI có nhiệm vụ sắp xếp, vá các lỗ hổng nội dung và biên soạn lại theo văn phong sử thi hoành tráng.
- **Quy tắc điều phối Mô hình (Tối ưu chi phí):**
  - *Mở đầu (Hook):* Gọi API trả phí của **Claude-3-5-Sonnet** để tạo ra 5 giây đầu tiên của video (Phần Hook) cực kỳ giật gân, ấn tượng sâu sắc và đoạn kết video kêu gọi hành động (Call to Action).
  - *Thân bài (Body):* Bàn giao (Handoff) nội dung thân bài sang cho **DeepSeek-V3/R1** xử lý để tiết kiệm Token tối đa khi xử lý khối lượng văn bản lớn.

### MODULE 2: BIÊN KỊCH LỜI THOẠI (Agent 2 - Worker)
- Nhận khung cảnh phân rã từ Agent 1, triển khai chi tiết lời thoại (Voiceover text). Văn phong bắt buộc phải hào hùng, trang nghiêm, nhịp điệu dồn dập phù hợp với định dạng Shorts sử thi.

### MODULE 3: ĐẠO DIỄN MỸ THUẬT AI (Agent 3 - Worker)
- Nhận lời thoại phân đoạn từ Agent 2, tự động tính toán thời lượng theo giây để tạo ra:
  1. Phân cảnh chi tiết (Storyboarding).
  2. Đoạn mã Prompt sinh ảnh sử thi hoành tráng.
  3. Gọi các công cụ tạo video AI mã nguồn mở (như MoviePy, GLTransitions hoặc các script điều khiển mô hình sinh video cục bộ) để chuyển đổi từ ảnh qua lời thoại thành video động hoàn chỉnh.

### MODULE 4: TỔNG BIÊN TẬP LỊCH SỬ & TỐI ƯU HÓA SEO (Agent 4 - Evaluator)
- **Kiểm duyệt Lịch sử:** Đối chiếu kịch bản thô với dữ liệu Kinh Thánh gốc để đảm bảo độ chính xác tuyệt đối của điển tích.
- **Kiểm duyệt Thuật toán:** Quét nội dung để đảm bảo cấu trúc Shorts chuẩn SEO. Tự động đóng gói sản phẩm đầu ra hoàn chỉnh gồm: Video ngắn + Tiêu đề giật gân (Title) + Mô tả chuẩn SEO (Description) + Thẻ Tags xu hướng.

## 3. Phong Cách Viết Code Yêu Cầu
- Luôn viết code dạng **Khung xương thô (Skeleton Code)** thể hiện rõ các class `Agent1_Orchestrator`, `Agent2_Scriptwriter`, `Agent3_ArtDirector`, `Agent4_Evaluator`.
- Sử dụng cơ chế `Handoff` trực tiếp từ **OpenAI Agents SDK** và bọc luồng phân cấp tuần tự bằng cấu trúc `Crew` của **CrewAI** để Copilot sinh code đồng bộ, không xung đột.
