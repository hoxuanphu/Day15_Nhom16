# Worksheet 4 – Production Readiness & Incident Response
# Họ và tên: Lại Đức Anh
# Mã sinh viên: 2A202600374

**Hệ thống:** Sử Việt AI Agent

---

### I. Tình huống 1: Traffic tăng đột biến (High Load)

**Ngữ cảnh:** Mùa thi hoặc sự kiện kỷ niệm khiến lượng truy cập tăng gấp 5-10 lần bình thường.

| Hành động | Giải pháp Ngắn hạn | Giải pháp Dài hạn |
| :--- | :--- | :--- |
| **Bảo vệ hệ thống** | **Rate Limiting:** Giới hạn 10 req/phút/IP. Vượt ngưỡng trả về HTTP 429. | **Auto-scaling:** Cấu hình HPA (Horizontal Pod Autoscaler) trên cụm Cloud. |
| **Xử lý backlog** | **Queue:** Đẩy request vào hàng đợi Redis/RabbitMQ, phản hồi người dùng bằng ticket "Đang xử lý". | **Async Processing:** Tách biệt hoàn toàn luồng nhận request và luồng xử lý Agent. |
| **Giảm tải** | **Graceful Degradation:** Chuyển sang Model Routing mặc định dùng model rẻ hơn để đảm bảo phục vụ. | **Semantic Caching** toàn diện. |

**Fallback Proposal:** Khi queue đầy hoặc quá tải, trả về thông báo: *"Hệ thống đang có lượng truy cập rất cao. Vui lòng thử lại sau 5 phút."*

---

### II. Tình huống 2: Provider Timeout / API LLM Lỗi

**Ngữ cảnh:** OpenAI hoặc Google API gặp sự cố mạng hoặc quá tải.

| Hành động | Giải pháp Ngắn hạn | Giải pháp Dài hạn |
| :--- | :--- | :--- |
| **Đảm bảo tính sẵn sàng** | **Retry với Exponential Backoff:** Thử lại 3 lần (1s, 2s, 4s). | **Multi-Provider:** Cấu hình fallback tự động (Primary: Gemini, Secondary: GPT-4o-mini). |
| **Cô lập sự cố** | **Circuit Breaker:** Nếu tỉ lệ lỗi > 50% trong 1 phút, tạm ngắt gọi API đó trong 30 giây. | **Health Check Endpoint** định kỳ. |
| **Trải nghiệm người dùng** | **Timeout Control:** Cài đặt deadline 10 giây cho toàn bộ luồng xử lý. | **Static Fallback:** Với câu hỏi phổ biến, trả về cache cứng nếu API chết hoàn toàn. |

**Fallback Proposal:** *"Trợ lý đang gặp chút khó khăn trong việc kết nối. Bạn vui lòng hỏi lại câu hỏi khác hoặc thử lại sau ít phút."*

---

### III. Tình huống 3: Response chậm (High Latency)

**Ngữ cảnh:** Câu hỏi yêu cầu Planner phân rã nhiều bước và truy xuất nhiều tài liệu.

| Hành động | Giải pháp Ngắn hạn | Giải pháp Dài hạn |
| :--- | :--- | :--- |
| **Tương tác UX** | **Streaming:** Trả kết quả từng token để người dùng không cảm thấy "chết treo". | **WebSocket Connection:** Duy trì kết nối hai chiều để cập nhật trạng thái Agent (Đang tìm kiếm -> Đang phân tích...). |
| **Tối ưu hóa** | **Giảm Chunk Size:** Giới hạn số lượng văn bản retrieve tối đa (Top K=3). | **Hybrid Search & Reranking:** Tăng độ chính xác của Retrieval để LLM xử lý ít token hơn. |
| **Xử lý nền** | Chuyển câu hỏi dài sang Async Queue, gửi email thông báo khi có kết quả. | Xây dựng tính năng "Thư viện câu hỏi của tôi". |

**Metrics cần Monitor:**
- `p95_latency_seconds`
- `retrieval_duration_ms`
- `token_usage_per_request`
- `cache_hit_rate`
