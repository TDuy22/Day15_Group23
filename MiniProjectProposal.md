# Mini Project Proposal – Enterprise Readiness Review

**Tên nhóm**: Nhóm 23  
**Chủ đề**: Chatbot hỗ trợ sale – Bán xe ô tô cho VinFast  
**Ngày**: 22/04/2026  

**Thành viên**:
- Nguyễn Thị Ngọc Thư (Product Lead & Presenter)
- Phạm Thành Duy (Architect)
- Phạm Xuân Khang (Cost Lead)
- Nguyễn Văn Thức (Reliability Lead)

### 1. Project Overview
Chatbot hỗ trợ sale VinFast là AI Agent giúp khách hàng tiềm năng và nhân viên sales tư vấn xe ô tô 24/7.  
Agent sử dụng RAG (kiến thức sản phẩm, giá, khuyến mãi) kết hợp tool calling (kiểm tra tồn kho, lịch hẹn, cập nhật CRM).  

**Bài toán chính**: Giảm thời gian tư vấn từ hàng giờ xuống dưới 30 giây, giảm tải cho đội sales và tăng tỷ lệ chuyển đổi lead.

**Người dùng chính**:
- Khách hàng cá nhân & doanh nghiệp quan tâm mua xe VinFast
- Nhân viên sales / tư vấn viên showroom

### 2. Enterprise Context & Ràng buộc
- **Tổ chức**: VinFast – doanh nghiệp ô tô lớn nhất Việt Nam (Vingroup)
- **Dữ liệu chính**: Thông tin xe, giá, khuyến mãi + PII khách hàng + lead CRM
- **Mức độ nhạy cảm**: Trung bình – Cao (PDPA + audit trail bắt buộc)
- **3 ràng buộc enterprise lớn nhất**:
  1. Tuân thủ PDPA & audit trail toàn bộ conversation
  2. Tích hợp legacy CRM/ERP nội bộ
  3. Data residency (dữ liệu ưu tiên lưu tại Việt Nam)

### 3. Deployment Choice
**Mô hình đề xuất**: **Hybrid**  
- **On-prem / Private VPC**: Xử lý PII khách hàng, lead, báo giá cá nhân hóa, audit log  
- **Cloud API**: RAG tư vấn sản phẩm + tool calling  
**Lý do chọn**: Đáp ứng PDPA & data residency đồng thời tối ưu tốc độ scale và chi phí.

### 4. Cost Analysis (MVP + Growth)
- **Traffic ước tính**: Bình thường 4.000–5.000 req/ngày | Peak 15.000–25.000 req/ngày
- **Cost driver lớn nhất**: LLM API Tokens (55–65%)
- **Cost MVP** (< 1.000 req/ngày): **$140 – $220 / tháng**
- **Cost Growth** (5.000–15.000 req/ngày): **$800 – $2.000 / tháng**

### 5. Cost Optimization Plan
**3 chiến lược tối ưu**:
1. **Semantic Caching** (làm ngay – MVP) → tiết kiệm 35–45% token
2. **Model Routing** (Growth) → tiết kiệm 40–50% token
3. **Prompt Compression + Selective Inference** (Scale) → tiết kiệm 20–30%

**Kết quả dự kiến**: Giảm 60–70% tổng token cost.

### 6. Reliability & Scaling Plan
**3 failure scenarios & giải pháp**:
1. Traffic peak → Auto-scaling + Queue + Semantic Cache
2. Provider timeout → Circuit Breaker + Multi-provider fallback
3. Confidence thấp / hallucination → Human escalation (chuyển lead cho sales)

**Metric monitor**: Latency p95 < 2.5s, Error rate < 2%, Confidence score, Lead conversion rate

**Nguyên tắc**: “Fail-safe + Human-in-the-loop” cho mọi giao dịch quan trọng.

### 7. Track Phase 2 Recommendation & Next Step
**Track đề xuất**: **AI Engineering & Production Track**  

**Lý do**: Nhóm mạnh AI Engineering, đã có use case enterprise rõ ràng (Hybrid deployment, cost optimization, reliability & scaling).

**Kỹ năng cần bổ sung**:
1. Advanced MLOps & Kubernetes
2. Enterprise Security & Compliance (PDPA, audit)
3. FinOps & Cost Governance

**Next step**: Xây production pipeline + triển khai semantic caching & model routing thực tế + security audit.

**Đề xuất**: Nhóm 23 rất mong muốn tiếp tục phát triển dự án Chatbot VinFast lên production-grade trong Phase 2.

---
**Nộp ngày**: 22/04/2026