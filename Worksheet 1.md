
* **Đối tượng sử dụng**:

  * Học sinh, sinh viên (tra cứu kiến thức)
  * Giáo viên, nhà nghiên cứu (tìm tài liệu chuyên sâu)
  * Người dùng phổ thông (hỏi nhanh thông tin lịch sử)
  * Du khách tham quan các khu di tích lịch sử (tìm hiểu thêm thông tin nơi mình đang tham quan)
* **Môi trường triển khai**:

  * Web app / mobile app / API tích hợp LMS (Learning Management System)
* **Yêu cầu sử dụng**:

  * Trả lời chính xác, có dẫn nguồn, không trả lời sai chủ đề hoặc sai trọng tâm dài dòng.
  * Agent không bị ảo giác
  * Phản hồi nhanh (real-time)
  * Có khả năng mở rộng khi nhiều user đồng thời

* Dữ liệu lịch sử:

  * Sách giáo khoa, tài liệu chính thống (đã qua kiểm duyệt)
  * Dữ liệu open-source (Wikipedia(chỉ để tham khảo, khuyến cáo không tin 100%), dataset lịch sử)
  * Tài liệu nội bộ (nếu có)
* Dữ liệu hệ thống:

  * Log truy vấn người dùng
  * Metadata (nguồn, thời gian, tác giả)
* Dữ liệu mô hình:

  * Embedding vectors
  * Index (FAISS, ElasticSearch,...)

* **Thấp và Trung bình**:

  * Dữ liệu lịch sử công khai -> thấp
* **Trung bình và Cao**:

    * Tài liệu bản quyền
    * Dữ liệu nội bộ tổ chức
    * Log người dùng (có thể chứa thông tin cá nhân) -> cao
    * Dữ liệu hệ thống -> cao
    * Cấu hình hệ thống -> cao

* **Ràng buộc enterprise lớn nhất**

    1. **Độ chính xác & kiểm chứng (Accuracy & Trust)**

        * Tránh hallucination
        * Phải có citation nguồn

    2. **Bảo mật & tuân thủ (Security & Compliance)**

        * Bảo vệ dữ liệu người dùng
        * Tuân thủ luật (GDPR-like, data privacy)

    3. **Khả năng mở rộng & hiệu năng (Scalability & Latency)**

        * Nhiều user đồng thời
        * Query vector search nhanh
        * Tối ưu chi phí compute

* **Đề xuất mô hình triển khai:** Hybrid Cloud (Kết hợp giữa Cloud và On-premise).
* **Lý do:**
    1. **Cân bằng giữa bảo mật và hiệu năng**

        * Dữ liệu nhạy cảm (log, tài liệu nội bộ) → On-prem
        * Model inference / scaling → Cloud

    2. **Tối ưu chi phí & mở rộng**

        * Cloud xử lý peak traffic (auto-scale)
        * On-prem giữ phần ổn định → giảm chi phí lâu dài

