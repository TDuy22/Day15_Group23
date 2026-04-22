# Worksheet 4 – Scaling & Reliability Tabletop

**Tên nhóm**: Nhóm 23  
**Ngày**: 22/04/2026  
**Chủ đề**: Chatbot hỗ trợ sale – Bán xe ô tô cho VinFast  

**Phân công vai trò nhóm**:
- Product Lead & Presenter: Nguyễn Thị Ngọc Thư  
- Architect: Phạm Thành Duy  
- Cost Lead: Phạm Xuân Khang  
- Reliability Lead: Nguyễn Văn Thức  

### 1. 3 tình huống failure scenarios chính

| Tình huống | Tác động tới user | Phản ứng ngắn hạn (immediate) | Giải pháp dài hạn |
|------------|-------------------|-------------------------------|-------------------|
| **1. Traffic tăng đột biến** (campaign marketing hoặc ra mắt mẫu xe mới – tăng 5–10x) | Khách hàng chờ lâu (>5s), bỏ cuộc, giảm chuyển đổi lead | Rate limiting + Queue (Redis) + trả lời “Đang xử lý, vui lòng chờ 10s” | Auto-scaling (Cloud Run / Kubernetes) + Semantic Caching + Predictive scaling dựa trên lịch campaign |
| **2. Provider timeout / LLM API error** (Claude / Grok outage) | Chatbot không phản hồi hoặc trả lỗi, khách hàng mất lòng tin | Circuit Breaker + Retry policy (3 lần) + Immediate fallback | Multi-provider routing (Grok + Claude + self-hosted fallback) + Health check dashboard |
| **3. Response chậm (high latency do RAG context lớn)** | Khách hàng cảm thấy chatbot kém thông minh, trải nghiệm kém, giảm tỷ lệ hoàn thành tư vấn | Timeout guard (max 4s) + trả câu trả lời ngắn “Đang tra cứu thêm thông tin…” | Prompt compression + Vector DB indexing tối ưu + Hybrid inference (cheap model cho câu đơn giản) |

### 2. Các metric cần monitor (đặt alert trên dashboard)
- **Latency**: p95 < 2.5 giây (real-time chat)
- **Error rate & Fallback rate**: < 2%
- **Queue length & Throughput**: requests/giây trong peak
- **Token cost / ngày** và **Cost per lead**
- **Confidence score** của LLM (đặc biệt với báo giá và lịch hẹn)
- **Lead conversion rate** (từ chatbot → sales)

### 3. Fallback proposal (chiến lược dự phòng)
- **Real-time request** (báo giá cá nhân hóa, lịch test drive): LLM chính (Claude Sonnet)  
- **Non-critical / lặp lại** (câu hỏi phổ biến về specs xe, khuyến mãi): Semantic Cache hoặc Rule-based answers  
- **Khi LLM fail hoặc confidence thấp** (< 70%):  
  - Chuyển ngay sang **human escalation** (gửi lead + tóm tắt cuộc trò chuyện cho nhân viên sales qua Slack/CRM)  
  - Hoặc fallback sang **rule-based + smaller model** (ví dụ Grok-3 hoặc self-hosted)  
- **Audit log**: Luôn ghi toàn bộ conversation + fallback action (bắt buộc cho PDPA)

**Gợi ý thảo luận nhóm** (đã xem xét):  
- Request nào cần real-time? → Báo giá & lịch hẹn.  
- Request nào có thể async? → Tư vấn specs xe, so sánh mẫu.  
- Cần queue / circuit breaker / retry? → **Có, bắt buộc**.  
- Fallback nên là model khác, rule-based hay human escalation? → Kết hợp cả ba theo mức độ nghiêm trọng.

**Kết luận reliability**:  
Hệ thống được thiết kế theo nguyên tắc “**Fail-safe + Human-in-the-loop**” cho mọi giao dịch quan trọng, đảm bảo không bao giờ để khách hàng thất vọng hoặc mất lead do lỗi AI.

**Hoàn thành lúc**: 11:25  
**Người chịu trách nhiệm chính**: Reliability Lead – Nguyễn Văn Thức