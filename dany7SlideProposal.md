# DÀN Ý 7 SLIDE PROPOSAL – Nhóm 23
**Chủ đề**: Chatbot hỗ trợ sale – Bán xe ô tô cho VinFast  
**Presenter**: Nguyễn Thị Ngọc Thư  
**Ngày**: 22/04/2026

### Slide 1: Tiêu đề & Project Overview
- **Tiêu đề**: Chatbot Hỗ trợ Sale VinFast – Enterprise Readiness Review
- **Tên nhóm**: Nhóm 23
- **Thành viên**: Nguyễn Thị Ngọc Thư (Product Lead & Presenter), Phạm Thành Duy (Architect), Phạm Xuân Khang (Cost Lead), Nguyễn Văn Thức (Reliability Lead)
- **Bài toán chính**: Tăng tốc độ tư vấn 24/7, giảm tải cho đội sales, nâng cao chuyển đổi lead cho khách hàng mua xe VinFast
- **User chính**:
  - Khách hàng tiềm năng (cá nhân & doanh nghiệp)
  - Nhân viên sales / tư vấn viên showroom
- **Giá trị kinh doanh**: Giảm thời gian chờ từ hàng giờ → dưới 30 giây, tăng tỷ lệ chuyển đổi lead

*(Hình ảnh: Logo VinFast + screenshot chatbot minh họa)*

### Slide 2: Bối cảnh Enterprise & Ràng buộc
- **Bối cảnh**: VinFast – doanh nghiệp ô tô lớn nhất Việt Nam (thuộc Vingroup)
- **Dữ liệu chính**: Thông tin xe, giá, khuyến mãi + PII khách hàng + lead CRM
- **Mức độ nhạy cảm**: Trung bình – Cao (PDPA + audit trail bắt buộc)
- **3 ràng buộc enterprise lớn nhất**:
  1. Tuân thủ PDPA & audit trail toàn bộ conversation
  2. Tích hợp legacy CRM/ERP nội bộ
  3. Data residency (dữ liệu phải ưu tiên lưu tại Việt Nam)
- **Kết luận**: Đây là hệ thống customer-facing thực tế của doanh nghiệp lớn → cần giải pháp production-grade.

*(Hình ảnh: Sơ đồ bối cảnh VinFast + icon PDPA, CRM, data residency)*

### Slide 3: Kiến trúc Triển khai Đề xuất
- **Mô hình đề xuất**: **Hybrid**
- **Chi tiết kiến trúc**:
  - **On-prem / Private VPC**: PII khách hàng, lead, báo giá cá nhân hóa, audit log
  - **Cloud API**: RAG tư vấn sản phẩm, tool calling (tồn kho, lịch hẹn)
- **Lý do chọn Hybrid**:
  1. Đáp ứng PDPA & data residency
  2. Tối ưu tốc độ scale + chi phí (non-sensitive part)
- **Trade-off**: Tăng độ phức tạp tích hợp nhưng đảm bảo an toàn & hiệu quả cao nhất

*(Hình ảnh: Diagram Hybrid Architecture – On-prem vs Cloud + secure API gateway)*

### Slide 4: Cost Anatomy (MVP + Growth)
- **Ước lượng traffic**:
  - Bình thường: 4.000–5.000 requests/ngày
  - Peak (campaign): 15.000–25.000 requests/ngày
- **Cost breakdown**:
  - LLM API Tokens: 55–65%
  - Compute/Hosting: 15–20%
  - Human review + Audit: 13–17%
- **Cost MVP** (< 1.000 req/ngày): **$140 – $220 / tháng**
- **Cost Growth** (5k–15k req/ngày): **$800 – $2.000 / tháng**
- **Cost driver lớn nhất**: LLM API Tokens

*(Hình ảnh: Biểu đồ cột Cost Breakdown + bảng MVP vs Growth)*

### Slide 5: Cost Optimization Plan
- **3 chiến lược tối ưu được chọn**:
  1. **Semantic Caching** (MVP) → tiết kiệm 35–45% token
  2. **Model Routing** (Growth) → tiết kiệm 40–50% token
  3. **Prompt Compression + Selective Inference** (Scale) → tiết kiệm 20–30%
- **Lộ trình**:
  - Làm ngay: Semantic Caching
  - Growth: Model Routing
  - Scale: Prompt Compression
- **Kết quả dự kiến**: Giảm 60–70% tổng token cost so với baseline

*(Hình ảnh: Bảng so sánh 3 chiến lược + biểu đồ tiết kiệm chi phí)*

### Slide 6: Reliability & Scaling
- **3 failure scenarios chính**:
  1. Traffic peak → Auto-scaling + Queue + Semantic Cache
  2. Provider timeout → Circuit Breaker + Multi-provider fallback
  3. Hallucination / confidence thấp → Human escalation
- **Metric monitor quan trọng**:
  - Latency p95 < 2.5s
  - Error rate < 2%
  - Confidence score & Lead conversion rate
- **Fallback strategy**: “Fail-safe + Human-in-the-loop” cho mọi giao dịch quan trọng

*(Hình ảnh: Flowchart Reliability Plan + bảng metric dashboard)*

### Slide 7: Track Phase 2 & Next Step
- **Track đề xuất**: **AI Engineering & Production Track**
- **Lý do chọn track**:
  - Nhóm mạnh AI Engineering + đã có use case enterprise rõ ràng
  - Hoàn toàn khớp với Hybrid deployment, cost optimization & reliability
- **Kỹ năng cần bổ sung**:
  1. Advanced MLOps & Kubernetes
  2. Enterprise Security & Compliance
  3. FinOps & Cost Governance
- **Next step Phase 2**:
  - Xây production pipeline + security audit
  - Triển khai semantic caching & model routing thực tế
  - Monitoring dashboard hoàn chỉnh

**Cảm ơn & Q&A**  
*(Hình ảnh: Timeline Phase 2 + badge “Best Enterprise Logic” hoặc “Best Cost Thinking”)*

**Tổng thời gian trình bày**: 5 phút



Hướng dẫn sử dụng nhanh cho Thư (Presenter):

Mỗi slide giữ 4–6 bullet points tối đa.
Dùng biểu đồ, diagram, bảng nhiều (đặc biệt slide 4, 5, 6) → điểm cao hơn rất nhiều.
Thời gian: Slide 1–2 (1 phút), 3–6 (3 phút), Slide 7 + Q&A (1 phút).