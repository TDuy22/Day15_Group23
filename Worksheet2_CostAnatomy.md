# Worksheet 2 – Cost Anatomy Lab

**Tên nhóm**: Nhóm 23  
**Ngày**: 22/04/2026  
**Chủ đề**: Chatbot hỗ trợ sale – Bán xe ô tô cho VinFast  

**Phân công vai trò nhóm**:
- Product Lead & Presenter: Nguyễn Thị Ngọc Thư  
- Architect: Phạm Thành Duy  
- Cost Lead: Phạm Xuân Khang  
- Reliability Lead: Nguyễn Văn Thức  

### 1. Ước lượng traffic
- **Số user/ngày bình thường**: 800 – 1.200 users  
- **Request/ngày bình thường**: 4.000 – 5.000 requests  
- **Peak traffic** (campaign marketing hoặc ra mắt mẫu xe mới): 15.000 – 25.000 requests/ngày (tăng 5–6 lần)

### 2. Ước lượng input/output tokens (giả sử dùng LLM API như Claude Sonnet / Grok)
- **Input tokens/request**: 800 – 1.200 tokens (lịch sử chat + RAG context về xe, giá, khuyến mãi)  
- **Output tokens/request**: 300 – 500 tokens (báo giá, tư vấn chi tiết, lịch hẹn)  
- **Tổng tokens/request trung bình**: ≈ **1.200 – 1.700 tokens**

### 3. Các lớp cost chính (Cost Breakdown)
| Lớp cost                  | Tỷ lệ ước tính | Ghi chú |
|---------------------------|----------------|---------|
| **API Tokens (LLM)**      | 55–65%        | Chiếm đa số, tăng tuyến tính với traffic |
| **Compute / Hosting (Hybrid)** | 15–20%     | Cloud Run + on-prem inference |
| **Storage (Vector DB + Cache)** | 5–8%       | Pinecone / Weaviate + Redis cache |
| **Human review & Escalation** | 8–10%      | Nhân viên sales review báo giá lớn |
| **Logging, Monitoring, Audit trail** | 5–7%   | Bắt buộc vì PDPA và audit bán hàng |

### 4. Tính sơ bộ cost ở mức MVP
- Traffic MVP: < 1.000 requests/ngày  
- Giá token (tham khảo Claude Sonnet): ~$3 / 1M input + $15 / 1M output  
- **Cost LLM API**: ≈ $80 – $110 / tháng  
- **Tổng cost MVP (Hybrid)**: **$140 – $220 / tháng** (đã bao gồm hosting, storage, logging)

### 5. Khi user tăng 5x hoặc 10x thì phần nào tăng mạnh nhất?
- Tăng **5x**: Token cost tăng ~5x (chiếm 65% → trở thành bottleneck)  
- Tăng **10x**: Token + Compute tăng mạnh nhất.  
  → Cần semantic cache và model routing để kiềm chế tăng trưởng chi phí.

### Câu hỏi nhóm đã trả lời
- **Cost driver lớn nhất của hệ thống là gì?**  
  **LLM API Tokens** (chiếm 55–65% tổng chi phí).

- **Hidden cost nào dễ bị quên nhất?**  
  **Human review & escalation** (khi chatbot báo giá lớn hoặc tự tin thấp) + **Audit logging** (bắt buộc lưu toàn bộ conversation để tuân thủ PDPA và trách nhiệm pháp lý).

- **Đội có chỗ nào đang ước lượng quá lạc quan không?**  
  Có: Chưa tính chi phí tích hợp legacy CRM/ERP và chi phí maintain Vector DB khi catalogue xe thay đổi thường xuyên.

**Hoàn thành lúc**: 10:15  
**Người chịu trách nhiệm chính**: Cost Lead – Phạm Xuân Khang