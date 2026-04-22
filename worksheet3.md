# Worksheet 3 – Cost Optimization Debate
**Hệ thống: Chatbot tra cứu lịch sử**

## Câu 1: 3 chiến lược phù hợp nhất
- Semantic Caching
- Model Routing
- Selective Inference / Phân tầng request


Vì sao loại 2 chiến lược còn lại:
- Prompt compression: domain lịch sử đòi hỏi độ chính xác cao về ngày tháng, tên người, địa danh, nén prompt dễ làm mất chi tiết then chốt, ROI thấp so với rủi ro.
- Smaller/self-hosted model: volume chưa đủ lớn để bù chi phí hạ tầng và rủi ro giảm accuracy trên domain chuyên biệt.

---

## Câu 2: Chi tiết từng chiến lược

### Chiến lược 1: Semantic Caching

**Tiết kiệm:** toàn bộ chuỗi Planner → N lần Executor → LLM synthesis cho mỗi cache hit.

**Lợi ích:**
- Domain rất hẹp (1965–1975) → độ tương đồng giữa các query cao → hit rate cao khi nhiều user hỏi.
- Sự thật lịch sử bất biến → cache không bị stale, không cần TTL ngắn như chatbot tin tức.
- Giảm cả chi phí token lẫn latency, quan trọng nếu dùng trong lớp học/demo.

**Trade-off:**
Phải dựng thêm embedding model + vector DB cho cache layer.
Rủi ro false positive: "trận Mậu Thân 1968" vs "thương vong trận Mậu Thân 1968" trông giống nhau nhưng intent khác → cần đặt ngưỡng similarity chặt + có fallback khi không chắc.

**Thời điểm áp dụng:** NGAY. Đây là quick win lớn nhất, không đụng logic core.

### Chiến lược 2: Model Routing

**Tiết kiệm:** chi phí mỗi lượt LLM call, bằng cách chọn model nhỏ hơn cho các bước không cần reasoning sâu.

**Lợi ích:**
Kiến trúc Planner-Executor đã sẵn có phân vai tự nhiên:

Planner (cần reasoning để chia nhỏ câu hỏi) → model lớn (ví dụ Sonnet/Opus).
Executor (tổng hợp từ đoạn văn đã retrieve) → model tầm trung hoặc nhỏ (Haiku).
Out-of-scope classifier → model rất nhỏ hoặc rule-based.


Không đánh đổi accuracy nếu phân vai đúng vì mỗi model làm đúng việc của mình.


**Trade-off:**

Pipeline phức tạp hơn (maintain nhiều model, nhiều prompt).
Cần benchmark lại: model nhỏ có đủ sức làm Executor trên văn bản sử Việt không (tên riêng, Hán-Việt, ngày âm lịch…).


**Thời điểm áp dụng:** NGAY. Khớp 1–1 với kiến trúc hiện có, chỉ đổi config model theo node, không thiết kế lại.

### Chiến lược 3: Selective Inference / Phân tầng request

**Tiết kiệm:** bỏ hẳn bước Planner (và đôi khi cả Executor) cho những câu hỏi không cần đến.

**Lợi ích:**

Hệ thống đã tự nhận diện được câu hỏi ngoài phạm vi → reject sớm bằng classifier rẻ, tiết kiệm 100% chi phí cho nhóm này (ví dụ: câu hỏi về đời sống hàng ngày).
Câu hỏi đơn giản (1 thực thể, 1 mốc thời gian) → đi thẳng RAG, bỏ qua Planning.
Câu hỏi phức tạp (đối chiếu nhiều mốc, nhiều trận, nhiều bên) → mới kích hoạt full Planner-Executor.


**Trade-off:**

Cần thêm classifier phân loại độ phức tạp → có thể phân loại sai.
Nếu sai về phía "đơn giản" cho câu thực ra phức tạp → chất lượng trả lời giảm, đụng đúng vào vấn đề mà Planner sinh ra để giải quyết.


**Thời điểm áp dụng:** SAU. Cần log dữ liệu thật từ người dùng để định nghĩa ngưỡng phân tầng; làm sớm dễ over-engineer với bộ 5 câu test.

---

## Câu 3: Chiến lược nào làm ngay, chiến lược nào để sau
**Làm ngay:**

- Semantic Caching — ROI cao nhất, implementation tách biệt khỏi core, phù hợp với tính bất biến của dữ liệu lịch sử.
- Model Routing — Khớp tự nhiên với Planner-Executor, chỉ cần đổi model per-node, hiệu quả thấy được ngay.

**Để sau:**

- Selective Inference / Phân tầng — làm sau khi có traffic thật và log đủ lớn để huấn luyện/đánh giá classifier phân loại độ phức tạp.