# Mini Project Proposal – Enterprise Readiness Review

**Tên dự án**: Chatbot Hỗ trợ Sale VinFast  
**User chính**: Khách hàng mua xe + Nhân viên sales  
**Bài toán**: Tăng tốc tư vấn & chuyển đổi lead 24/7

### 1. Bối cảnh Enterprise & Ràng buộc
- Doanh nghiệp lớn Việt Nam, dữ liệu PII + kinh doanh nhạy cảm
- 3 ràng buộc: PDPA, legacy CRM, data residency

### 2. Kiến trúc Triển khai Đề xuất: **Hybrid**
- On-prem/VPC: PII + CRM integration
- Cloud: RAG + tool calling

### 3. Cost Analysis
- MVP (<1k req/ngày): $120–180/tháng
- Growth (5k–15k req/ngày): $800–2.000/tháng

### 4. Cost Optimization
- Semantic Caching, Model Routing, Prompt Compression

### 5. Reliability & Scaling
- Auto-scaling, queue, fallback rule-based + human escalation
- Monitor: latency, error rate, confidence score

### 6. Track Phase 2 & Next Step
**AI Engineering & Production Track**  
Next step: Xây production pipeline + security audit

**Nhóm đề xuất**: [Tên nhóm] – Ngày 22/04/2026