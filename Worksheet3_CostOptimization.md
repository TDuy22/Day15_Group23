# Worksheet 3 – Cost Optimization Debate

**Tên nhóm**: Nhóm 23  
**Ngày**: 22/04/2026  
**Chủ đề**: Chatbot hỗ trợ sale – Bán xe ô tô cho VinFast  

**Phân công vai trò nhóm**:
- Product Lead & Presenter: Nguyễn Thị Ngọc Thư  
- Architect: Phạm Thành Duy  
- Cost Lead: Phạm Xuân Khang  
- Reliability Lead: Nguyễn Văn Thức  

### 1. 3 chiến lược tối ưu chi phí phù hợp nhất (đã chọn sau khi thảo luận)

| STT | Chiến lược                  | Tiết kiệm ước tính | Lợi ích chính | Trade-off | Thời điểm áp dụng |
|-----|-----------------------------|--------------------|---------------|-----------|-------------------|
| 1   | **Semantic Caching**        | 35–45% token cost | Giảm lặp lại câu hỏi phổ biến (so sánh xe, giá cơ bản, chính sách bảo hành) | Cache invalidation khi giá/khuyến mãi thay đổi | **Làm ngay (MVP)** |
| 2   | **Model Routing**           | 40–50% tổng token cost | Dùng cheap model (Claude Haiku / Grok-3) cho câu hỏi đơn giản; premium model (Claude Sonnet) chỉ cho báo giá phức tạp hoặc lead cao | Cần thêm logic routing + confidence score | **Growth phase** (khi > 5.000 req/ngày) |
| 3   | **Prompt Compression + Selective Inference** | 20–30% token + latency | Nén RAG context + chỉ gọi LLM đầy đủ cho request quan trọng (báo giá > 500 triệu hoặc lead scoring cao) | Phức tạp hơn ở tầng orchestration | **Scale phase** (khi > 15.000 req/ngày) |

### 2. Chi tiết từng chiến lược

**Chiến lược 1: Semantic Caching (ưu tiên làm ngay)**  
- **Tiết kiệm phần nào**: LLM API Tokens (chiếm 55–65% tổng cost).  
- **Lợi ích**: Hầu hết câu hỏi của khách hàng là lặp lại (ví dụ: “Xe VF8 có mấy phiên bản?”, “Giá VF e34 tháng này bao nhiêu?”). Cache semantic sẽ trả lời ngay mà không gọi LLM.  
- **Trade-off**: Phải có cơ chế invalidate cache khi catalogue xe hoặc khuyến mãi thay đổi (có thể dùng TTL + webhook).  
- **Thời điểm**: Áp dụng ngay từ MVP vì triển khai đơn giản (Redis / Vector DB).

**Chiến lược 2: Model Routing**  
- **Tiết kiệm phần nào**: Token cost + latency.  
- **Lợi ích**: 80% request là câu hỏi đơn giản → dùng model rẻ & nhanh; chỉ 20% request phức tạp (báo giá cá nhân hóa, so sánh nhiều xe) mới gọi model mạnh.  
- **Trade-off**: Cần thêm orchestrator layer để phân loại intent và confidence score.  
- **Thời điểm**: Growth phase khi traffic ổn định và có đủ dữ liệu để train router.

**Chiến lược 3: Prompt Compression + Selective Inference**  
- **Tiết kiệm phần nào**: Token input + chi phí compute.  
- **Lợi ích**: Nén lịch sử chat + RAG context trước khi gửi LLM; chỉ gọi full inference cho request có giá trị kinh doanh cao.  
- **Trade-off**: Tăng độ phức tạp code và cần monitoring chặt chẽ.  
- **Thời điểm**: Scale phase khi cost đã trở thành bottleneck lớn.

### 3. Kết luận & Lộ trình áp dụng
- **Làm ngay (MVP)**: Semantic Caching → tiết kiệm nhanh nhất, dễ triển khai.  
- **Làm sau (Growth)**: Model Routing.  
- **Scale**: Prompt Compression + Selective Inference.  

**Cost driver lớn nhất sau tối ưu**: Vẫn là LLM Tokens, nhưng giảm được 60–70% so với baseline.

**Hoàn thành lúc**: 10:50  
**Người chịu trách nhiệm chính**: Cost Lead – Phạm Xuân Khang