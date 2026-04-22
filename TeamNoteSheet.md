# Team Note Sheet – Ngày tổng kết Phase 1

**Tên nhóm**: Nhóm 23  
**Chủ đề**: Chatbot hỗ trợ sale – Bán xe ô tô cho VinFast

**3 ràng buộc enterprise lớn nhất**:
1. Tuân thủ PDPA & audit trail bắt buộc cho mọi tương tác bán hàng
2. Tích hợp hệ thống legacy CRM/ERP nội bộ VinFast
3. Data residency (ưu tiên lưu trữ dữ liệu khách hàng tại Việt Nam)

**Cost driver lớn nhất**:  
**LLM API Tokens** (chiếm 55–65% tổng chi phí)

**3 chiến lược optimize được chọn**:
1. **Semantic Caching** (làm ngay – MVP) → tiết kiệm 35–45% token
2. **Model Routing** (Growth phase) → tiết kiệm 40–50% token
3. **Prompt Compression + Selective Inference** (Scale phase) → tiết kiệm 20–30% token & latency

**Fallback / reliability plan ngắn gọn**:
- Traffic peak → Auto-scaling + Queue + Semantic Cache
- LLM error → Circuit Breaker + Multi-provider fallback
- Confidence thấp hoặc báo giá lớn → Human escalation (chuyển lead cho sales qua CRM)
- Luôn monitor latency p95 < 2.5s, error rate < 2%, confidence score

**Track Phase 2 đề xuất**:  
**AI Engineering & Production Track**

**Lý do**: Nhóm mạnh AI Engineering + đã có use case enterprise rõ ràng (Hybrid deployment, cost optimization, reliability). Track này giúp đưa chatbot VinFast lên production-grade thực tế.

**Hoàn thành lúc**: 11:45  
**Người chịu trách nhiệm**: Toàn nhóm