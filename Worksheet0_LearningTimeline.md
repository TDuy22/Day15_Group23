# Worksheet 0 – Learning Timeline & Chủ đề

**Tên nhóm**: Nhóm 23  
**Ngày**: 22/04/2026  
**Chủ đề được chọn**: Chatbot hỗ trợ sale – Bán xe ô tô cho VinFast

**Phân công vai trò nhóm**:
- Product Lead & Presenter: Nguyễn Thị Ngọc Thư  
- Architect: Phạm Thành Duy  
- Cost Lead: Phạm Xuân Khang  
- Reliability Lead: Nguyễn Văn Thức  

### 2–3 kỹ năng nhóm tự tin nhất sau 15 ngày Phase 1
1. Xây dựng và triển khai AI Agent hoàn chỉnh (tool calling + RAG pipeline)  
2. Thiết kế UX chatbot, evaluation chất lượng phản hồi và monitoring production  
3. Nhận diện cost driver, safety guardrails và basic reliability patterns  

### Mô tả ngắn sản phẩm / agent
**Chatbot hỗ trợ sale VinFast** là một AI Agent hỗ trợ bộ phận Kinh doanh ô tô.  
Agent giúp:
- Tư vấn mẫu xe, so sánh tính năng, báo giá cá nhân hóa
- Đặt lịch test drive, tra cứu tồn kho thời gian thực
- Xử lý lead và chuyển thông tin chất lượng cao vào hệ thống CRM nội bộ

Agent sử dụng **RAG** (kiến thức về xe, giá, khuyến mãi, chính sách) kết hợp **tool calling** (kiểm tra tồn kho, lịch hẹn, cập nhật CRM).

### Câu hỏi nhóm đã trả lời (theo yêu cầu Hoạt động 1)

- **Sản phẩm này giải quyết bài toán gì?**  
  Tăng tốc độ tư vấn khách hàng 24/7, giảm tải công việc lặp lại cho đội ngũ sales (hàng nghìn câu hỏi mỗi ngày), nâng cao tỷ lệ chuyển đổi lead và cải thiện trải nghiệm mua xe cho khách hàng.

- **Ai là người dùng chính?**  
  - Khách hàng tiềm năng (cá nhân và doanh nghiệp quan tâm mua xe VinFast)  
  - Nhân viên sales / tư vấn viên tại showroom và kênh online

- **Vì sao chủ đề này phù hợp để phân tích deployment và cost?**  
  Đây là hệ thống **customer-facing** thực tế của một doanh nghiệp lớn Việt Nam.  
  Có traffic biến động mạnh (campaign marketing, ra mắt mẫu xe mới), dữ liệu khách hàng nhạy cảm (PII), cần tích hợp legacy CRM/ERP, và đòi hỏi độ tin cậy cao vì liên quan trực tiếp đến doanh thu và uy tín thương hiệu.  
  Do đó rất phù hợp để phân tích sâu về Hybrid deployment, cost anatomy, optimization và reliability ở quy mô enterprise.

**Hoàn thành lúc**: 08:40  
**Người chịu trách nhiệm chính**: Product Lead – Nguyễn Thị Ngọc Thư