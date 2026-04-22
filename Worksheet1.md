* Họ và tên: Hồ xuân Phú
* MSHV: 2A202600061

### Chatbot tra cứu lịch sử
* **Đối tượng sử dụng**:

  * Học sinh, sinh viên (tra cứu kiến thức)
  * Giáo viên, nhà nghiên cứu (tìm tài liệu chuyên sâu)
  * Người dùng phổ thông (hỏi nhanh thông tin lịch sử)
  * Du khách tham quan các khu di tích lịch sử (tìm hiểu thêm thông tin nơi mình đang tham quan)
* **Môi trường triển khai**:

  * Web app / mobile app / API tích hợp LMS (Learning Management System)
* **Yêu cầu sử dụng**:

  * Trả lời chính xác, có dẫn nguồn, không trả lời sai chủ đề hoặc sai trọng tâm dài dòng.
  * Trích dẫn đúng nguồn tài liệu
  * Agent không bị ảo giác
  * Phản hồi nhanh (real-time)
  * Có khả năng mở rộng khi nhiều user đồng thời

* **Dữ liệu hệ thống sử dụng**
    * Dữ liệu lịch sử:

        * Sách giáo khoa, tài liệu chính thống (đã qua kiểm duyệt)
        * Dữ liệu open-source (Wikipedia(chỉ để tham khảo, khuyến cáo không tin 100%), dataset lịch sử)
        * Tài liệu nội bộ (nếu có)
        * Tài liệu nội bọ hạn chế truy cập (nếu có)
    * Dữ liệu hệ thống:

        * Log truy vấn người dùng
        * Metadata (nguồn, thời gian, tác giả)
        * Dữ liệu mô hình:

        * Embedding vectors
        * Index (FAISS, ElasticSearch,...)

    * Thấp và Trung bình**:

        * Dữ liệu lịch sử công khai -> thấp
    * Trung bình và Cao**:

        * Tài liệu bản quyền
        * Dữ liệu nội bộ tổ chức -> trung bình
        * Log người dùng (có thể chứa thông tin cá nhân) -> cao
        * Dữ liệu hệ thống -> cao
        * Cấu hình hệ thống -> cao
        * Tài liệu nội bộ hạn chế truy cập -> rất cao
     * Thường xuyên kiểm tra dữ liệu, câu trả lời của chatbot
     * Cần có hội đồng kiểm duyệt có chuyên môn trước khi đưa vào hệ thống.
* **Ràng buộc enterprise lớn nhất**
    * Tính kế thừa và Tích hợp: Phải tích hợp được với hệ thống quản lý dữ liệu hiện có của tổ chức (Legacy systems) và tuân thủ quy trình phê duyệt nội dung trước khi câu trả lời được hiển thị.

    * Khả năng truy vết (Audit Trail): Mọi câu hỏi và câu trả lời phải được ghi lại để kiểm toán. Nếu AI trả lời sai gây hiểu lầm về lịch sử, tổ chức phải biết rõ nguyên nhân do dữ liệu nguồn hay lỗi mô hình.

    * Chủ quyền dữ liệu (Data Sovereignty): Dữ liệu không được phép rời khỏi biên giới tổ chức hoặc biên giới quốc gia (không sử dụng các Public API của nước ngoài cho dữ liệu mật).

* **Đề xuất mô hình triển khai:** Hybrid Cloud (Kết hợp giữa Cloud và On-premise).
* **Lý do:**
    1. **Cân bằng giữa bảo mật và hiệu năng**

        * Dữ liệu nhạy cảm (log, tài liệu nội bộ) → On-prem
        * Model inference / scaling → Cloud

    2. **Tối ưu chi phí & mở rộng**

        * Cloud xử lý peak traffic (auto-scale)
        * On-prem giữ phần ổn định → giảm chi phí lâu dài

