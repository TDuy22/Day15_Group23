# Worksheet 1 – Enterprise Deployment Clinic

**Tên nhóm**: Nhóm 23  
**Chủ đề**: Chatbot hỗ trợ sale – Bán xe ô tô cho VinFast  
**Ngày**: 22/04/2026  

**Phân công vai trò nhóm**:
- Product Lead & Presenter: Nguyễn Thị Ngọc Thư  
- Architect: Phạm Thành Duy  
- Cost Lead: Phạm Xuân Khang  
- Reliability Lead: Nguyễn Văn Thức  

### 1. Bối cảnh tổ chức / khách hàng sử dụng hệ thống
VinFast là doanh nghiệp ô tô hàng đầu Việt Nam, thuộc Tập đoàn Vingroup.  
Chatbot được triển khai nhằm hỗ trợ bộ phận Kinh doanh & Tiếp thị, phục vụ hai nhóm người dùng chính:
- **Khách hàng tiềm năng** (cá nhân và doanh nghiệp): tư vấn mẫu xe, báo giá, so sánh tính năng, đặt lịch test drive, trả lời chính sách bảo hành, vay trả góp…
- **Nhân viên sales / tư vấn viên**: hỗ trợ tra cứu thông tin thời gian thực (tồn kho, khuyến mãi, hợp đồng mẫu), chuyển lead chất lượng cao vào hệ thống CRM.

Mục tiêu kinh doanh:  
- Tăng tốc độ tư vấn 24/7, giảm thời gian chờ của khách hàng từ vài giờ xuống dưới 30 giây.  
- Tăng tỷ lệ chuyển đổi lead và giảm tải công việc cho đội ngũ sales (hiện đang phải xử lý hàng nghìn câu hỏi lặp lại mỗi ngày).

### 2. Dữ liệu mà hệ thống sẽ động đến
- **Dữ liệu sản phẩm**: Thông số kỹ thuật xe, giá bán, khuyến mãi theo khu vực, tồn kho thời gian thực, lịch giao xe.
- **Dữ liệu khách hàng (PII)**: Họ tên, số điện thoại, email, địa chỉ, lịch sử quan tâm xe, nhu cầu tài chính, hồ sơ test drive.
- **Dữ liệu giao dịch**: Báo giá cá nhân hóa, lịch hẹn test drive, hợp đồng tạm, lead scoring.
- **Dữ liệu nội bộ**: Thông tin CRM/ERP cũ của VinFast (hệ thống quản lý bán hàng, kho vận, tài chính).

### 3. Đánh giá mức độ nhạy cảm của dữ liệu
**Mức độ: Trung bình – Cao**  
- Dữ liệu cá nhân khách hàng (PII) thuộc diện bảo vệ theo **PDPA** (Luật Bảo vệ dữ liệu cá nhân).  
- Dữ liệu kinh doanh (giá xe, khuyến mãi, tồn kho) mang tính nhạy cảm thương mại.  
- Mọi tương tác của chatbot đều cần **audit trail** để truy vết trách nhiệm khi có khiếu nại hoặc tranh chấp hợp đồng.  
→ Không phải dữ liệu cực nhạy như y tế hay ngân hàng, nhưng vẫn đòi hỏi mức bảo mật cao vì liên quan trực tiếp đến giao dịch tài chính và uy tín thương hiệu.

### 4. 3 ràng buộc enterprise lớn nhất
1. **Tuân thủ pháp lý & bảo mật** (PDPA, audit trail bắt buộc cho mọi câu trả lời liên quan đến giá, hợp đồng, cam kết).
2. **Tích hợp hệ thống legacy** (phải kết nối với CRM/ERP nội bộ hiện tại của VinFast mà không làm gián đoạn vận hành).
3. **Data residency & sovereignty** (dữ liệu khách hàng Việt Nam ưu tiên lưu trữ tại Việt Nam, hạn chế đưa toàn bộ lên public cloud nước ngoài).

### 5. Mô hình triển khai đề xuất: **Hybrid**

### 6. 2 lý do chính chọn Hybrid (logic & trade-off rõ ràng)
**Lý do 1**:  
Phần dữ liệu nhạy cảm (PII khách hàng, lead, báo giá cá nhân hóa, audit log) sẽ được lưu và xử lý trên **on-prem / Private Cloud / VPC riêng** của VinFast. Điều này đảm bảo tuân thủ PDPA, data residency và kiểm soát hoàn toàn quyền truy cập – giảm rủi ro tuân thủ pháp lý.

**Lý do 2**:  
Phần RAG (kiến thức sản phẩm, specs xe, khuyến mãi chung) và tool calling (kiểm tra tồn kho, lịch hẹn) có thể sử dụng **Cloud API** (nhanh, scale tự động, chi phí linh hoạt). Kiến trúc này cho phép hybrid routing: dữ liệu nhạy cảm không rời khỏi hạ tầng nội bộ, trong khi phần non-sensitive tận dụng tốc độ và chi phí thấp của cloud.

**Trade-off được chấp nhận**:  
- Tăng độ phức tạp tích hợp (cần secure API gateway).  
- Nhưng đổi lại là sự cân bằng hoàn hảo giữa **bảo mật enterprise** và **tốc độ scale + chi phí hợp lý**.

**Gợi ý thảo luận nhóm** (đã xem xét):  
- Dữ liệu có được rời khỏi tổ chức không? → Phần PII và báo giá **không**.  
- Có cần audit trail không? → **Bắt buộc 100%**.  
- Nếu chatbot trả lời sai (ví dụ báo giá sai) thì ai bị ảnh hưởng đầu tiên? → Khách hàng → Nhân viên sales → Uy tín thương hiệu VinFast.

**Hoàn thành lúc**: 09:25  
**Người chịu trách nhiệm chính**: Architect – Phạm Thành Duy