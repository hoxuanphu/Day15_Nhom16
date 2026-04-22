# Worksheet 5 – Skills Map & Track Direction

**Dự án:** Sử Việt AI Agent

---

### 1. Bảng Tự Đánh giá Năng lực Nhóm (Scale 1-5)

| Thành viên | Business / Product | Infra / Data / Ops | AI Engineering / Application |
| :--- | :---: | :---: | :---: |
| Đào Danh Đăng Phụng | 2 | 2 | 3 |
| Hồ Xuân Phú | 2 | 4 | 5 |
| Phạm Anh Quân | 3 | 3 | 4 |
| Hoàng Ngọc Thạch | 2 | 2 | 2 |
| Nguyễn Minh Trí | 3 | 3 | 4 |
| Lại Đức Anh | 3 | 3 | 4 |
| **Điểm mạnh chung của nhóm** | Có kiến thức cơ bản về yêu cầu giáo dục. | Có khả năng vận hành hạ tầng (Hybrid). | **Rất mạnh:** Thiết kế Agent, RAG và Evaluation. |

---

### 2. Lựa chọn Track Phase 2

- **Track đã chọn:** **AI Application (Advanced RAG & Agentic Workflow)**

- **Lý do phù hợp với dự án:**  
  - Sản phẩm hiện tại đã có kiến trúc ReAct Agent và RAG cơ bản.  
  - Để đưa vào Production, cần nâng cấp lên **Hybrid Search**, **Query Decomposition** và **Long-term Memory** cho hội thoại nhiều lượt.  
  - Tận dụng tối đa thế mạnh sẵn có của nhóm về AI Engineering.

---

### 3. Kỹ năng cần bổ sung để Project thành công

1. **LLMOps & Production Observability:**  
   - *Mục tiêu:* Thành thạo công cụ như **Langfuse** hoặc **Arize Phoenix**.  
   - *Ứng dụng:* Theo dõi chi tiết từng Trace của Agent, phát hiện sớm bottleneck hoặc hallucination trong môi trường thật.

2. **Hybrid Search & Advanced Retrieval:**  
   - *Mục tiêu:* Kết hợp **BM25** (Từ khóa chính xác) với **Dense Vector** (Ngữ nghĩa) để tìm kiếm chính xác các mốc thời gian và tên riêng Hán-Việt.  
   - *Ứng dụng:* Khắc phục điểm yếu của Vector Search thuần túy với các con số và ngày tháng.

3. **Product Analytics & User Feedback Loop:**  
   - *Mục tiêu:* Phân tích tỉ lệ "thumbs up/down" để cải thiện chất lượng Golden Dataset.  
   - *Ứng dụng:* Xác định đâu là câu hỏi khó để ưu tiên cải thiện mô hình hoặc bổ sung tài liệu.
