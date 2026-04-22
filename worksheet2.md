# Worksheet 2 - Cost Anatomy Lab


**Sản phẩm:** Sử Việt AI Agent  
**Mô hình triển khai:** Hybrid Cloud

---

### 1. Ước lượng Lưu lượng Người dùng (MVP)

- **Người dùng ước tính / ngày:** **300 Users**
- **Số request trung bình / user / ngày:** **4 Requests**
- **Tổng request / ngày:** **1.200 Requests**
- **Phân bố tải cao điểm:** 30% lưu lượng tập trung vào 3 giờ cao điểm (khoảng 120 req/giờ, tương đương 2 req/phút).

---

### 2. Ước lượng Token Usage (LLM API)

Với kiến trúc Planner-Executor (RAG + Agent):

- **Input Tokens trung bình / request:** **800 tokens** (bao gồm prompt hệ thống, ngữ cảnh lịch sử truy xuất được).
- **Output Tokens trung bình / request:** **250 tokens** (câu trả lời tóm tắt, có dẫn nguồn).

| Thông số | Tính toán | Kết quả |
| :--- | :--- | :--- |
| Tổng Input / ngày | 800 × 1.200 | 960.000 tokens |
| Tổng Output / ngày | 250 × 1.200 | 300.000 tokens |
| **Tổng Token / tháng (30 ngày)** | (1.260.000) × 30 | **37.800.000 tokens** |

---

### 3. Chi phí Token (Tham chiếu giá OpenAI GPT / Gemini tương đương)

| Model | Input ($/1M) | Output ($/1M) | Chi phí / ngày | Chi phí / tháng |
| :--- | :---: | :---: | :---: | :---: |
| **GPT-5.4** (Mạnh nhất) | $2.50 | $15.00 | ~ $4.20 | **~ $126** |
| **GPT-5.4-Mini** | $0.75 | $4.50 | ~ $1.03 | **~ $31** |
| **GPT-4o-mini** | $0.15 | $0.60 | ~ $0.22 | **~ $6.6** |
| **Gemini 1.5 Flash** | $0.075 | $0.30 | ~ $0.16 | **~ $5** |

---

### 4. Các Lớp Chi phí Khác (Hidden Cost)

| Hạng mục | Chi tiết | Ước lượng MVP/tháng |
| :--- | :--- | :---: |
| **Compute (Cloud/On-prem)** | Backend API, Queue Worker, Vector DB | ~ $20 - $50 |
| **Storage & Database** | Logs, Traces, Vector Embeddings | ~ $5 - $10 |
| **Human Review** | Duyệt 5% câu hỏi khó hoặc phản hồi tiêu cực (60 ca/ngày) | ~ $150 - $300 (nhân công) |
| **Logging & Monitoring** | Langfuse, Prometheus, Cloud Logging | ~ $10 - $30 |

---

### 5. Trả lời câu hỏi Cost Driver

**Q1: Cost driver lớn nhất là gì?**  
- Ở giai đoạn MVP với traffic thấp, **chi phí LLM Token** vẫn là khoản chi chính nếu sử dụng model lớn.  
- Khi traffic tăng 10x, **chi phí nhân công kiểm duyệt (Human Review)** có thể vượt mặt chi phí máy móc nếu chất lượng AI chưa hoàn thiện.

**Q2: Hidden cost dễ bị quên nhất?**  
- **Chi phí Retry & Timeout:** Khi provider lỗi, việc gọi lại API nhiều lần làm đội token usage lên cao đột biến.  
- **Logging Volume:** Chi phí lưu trữ và truy vấn log cho mục đích Audit Trail dễ bị bỏ qua khi thiết kế.

**Q3: Đội có đang ước lượng lạc quan không?**  
- **Có.** Việc giả định 800 input tokens là tương đối thấp đối với Agent cần Planner-Executor nhiều bước và ngữ cảnh lịch sử dài. Thực tế có thể lên đến **2.000 - 4.000 tokens/request** khi truy xuất nhiều tài liệu.  
- Cần lập kế hoạch **tối ưu ngay từ đầu** để tránh bội chi.
