* Họ và tên: Hồ xuân Phú
* MSHV: 2A202600061

### Chatbot tra cứu lịch sử
**Sản phẩm:** Sử Việt AI Agent

---

### 1. Bối cảnh tổ chức & Người dùng mục tiêu

- **Tổ chức giả định:** Một đơn vị giáo dục hoặc viện nghiên cứu lịch sử, mong muốn cung cấp công cụ tra cứu thông minh cho học sinh, sinh viên và cán bộ nội bộ.
- **Người dùng chính:**
  - Học sinh, sinh viên (tra cứu nhanh, ôn tập).
  - Giáo viên, nhà nghiên cứu (kiểm chứng dữ liệu, tổng hợp tài liệu).
  - Du khách tham quan bảo tàng/di tích (tìm hiểu thông tin tại chỗ).

---

### 2. Dữ liệu hệ thống & Mức độ nhạy cảm

| Loại dữ liệu | Ví dụ cụ thể | Mức độ nhạy cảm |
| :--- | :--- | :--- |
| **Dữ liệu lịch sử công khai** | Sách giáo khoa, tài liệu chính thống đã kiểm duyệt, Wikipedia (tham khảo) | **Thấp** |
| **Tài liệu nội bộ / bản quyền** | Tư liệu lưu trữ riêng của tổ chức, bài báo khoa học | **Trung bình** |
| **Log truy vấn người dùng** | Lịch sử câu hỏi, địa chỉ IP, hành vi sử dụng | **Cao** (có thể chứa thông tin cá nhân) |
| **Cấu hình hệ thống & Metadata** | Index vector, cấu trúc DB, tài liệu hạn chế truy cập | **Rất cao** |

---

### 3. Ba ràng buộc Enterprise quan trọng nhất

1. **Chủ quyền Dữ liệu (Data Sovereignty):**  
   Dữ liệu lịch sử nội bộ và log người dùng **không được phép rời khỏi biên giới quốc gia**. Không sử dụng các public API nước ngoài cho các tác vụ liên quan đến dữ liệu nhạy cảm.

2. **Khả năng Truy vết (Audit Trail):**  
   Mọi câu hỏi – câu trả lời đều phải được lưu trữ để phục vụ kiểm toán. Khi có thông tin sai lệch, tổ chức cần xác định được nguyên nhân gốc rễ (do nguồn tài liệu hay do mô hình).

3. **Quy trình Phê duyệt & Tích hợp:**  
   Hệ thống phải tích hợp được với kho dữ liệu có sẵn (Legacy systems) và có cơ chế để chuyên gia duyệt mẫu câu trả lời trước khi công bố rộng rãi.

---

### 4. Đề xuất Mô hình Triển khai: **Hybrid Cloud**

**Lý do lựa chọn:**

1. **Cân bằng Bảo mật & Hiệu năng:**  
   - **On-premise:** Lưu trữ dữ liệu nhạy cảm (log, tài liệu mật), đảm bảo chủ quyền dữ liệu.  
   - **Cloud (Private Cloud nội địa):** Triển khai inference LLM và auto-scaling để xử lý tải cao điểm mà không cần đầu tư hạ tầng cứng quá lớn.

2. **Tối ưu Chi phí Vận hành Dài hạn:**  
   Giữ workload ổn định trên hạ tầng on-prem để tiết kiệm chi phí thuê cloud thường xuyên, đồng thời tận dụng khả năng co giãn của cloud khi có đợt cao điểm (ví dụ: mùa thi, lễ kỷ niệm lịch sử).

    2. **Tối ưu chi phí & mở rộng**

        * Cloud xử lý peak traffic (auto-scale)
        * On-prem giữ phần ổn định → giảm chi phí lâu dài

