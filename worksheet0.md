# Worksheet 0

**Họ và Tên:** Nguyễn Minh Trí  
**MSHV:** 2A202600182 

---

### Mục tiêu:
Xác định nhóm đã xây dựng được gì trong 15 ngày qua và lựa chọn đúng dự án/chủ đề để thực hiện phân tích production.

---

### I. Nhóm cần làm gì?

#### 1. Liệt kê 2–3 kỹ năng nhóm tự tin nhất:
- **Thiết kế & Triển khai ReAct Agent:** Thành thạo trong việc xây dựng chu trình Thought-Action-Observation, giúp AI có khả năng lập luận và sử dụng công cụ để giải quyết các Task phức tạp.
- **Xây dựng Hệ thống Tools & Retrieval chuyên sâu:** Phát triển các bộ công cụ tùy chỉnh (Timeline extraction, Entity lookup) và tích hợp Hybrid Retriever để tối ưu hóa việc truy xuất dữ liệu chính xác.
- **Observability & Performance Analytics:** Có kinh nghiệm trong việc thiết lập hệ thống telemetry, phân tích log và các chỉ số vận hành (Latency, Token usage, Cost) để cải thiện chất lượng sản phẩm.

#### 2. Mô tả ngắn sản phẩm/agent nhóm đã làm:
**Sản phẩm:** **Sử Việt AI Agent**  
Đây là một trợ lý ảo thông minh chuyên sâu về lịch sử Việt Nam giai đoạn 1965–1975. Khác với Chatbot thông thường chỉ trả lời dựa trên dữ liệu có sẵn, Agent này sử dụng kiến trúc Planner-Executor để phân tích các câu hỏi khó (ví dụ: so sánh các mốc thời gian, tổng hợp số liệu), từ đó đưa ra câu trả lời có tính hệ thống, minh bạch và giảm thiểu hiện tượng hallucination.

#### 3. Chốt 1 chủ đề sẽ theo xuyên suốt cả ngày:
**Chủ đề:** Đưa Sử Việt AI Agent vào môi trường Production: Tối ưu hóa Độ tin cậy (Reliability), Độ trễ (Latency) và Chi phí vận hành (Cost).

---

### II. Câu hỏi nhóm phải trả lời:

#### 1. Sản phẩm này giải quyết bài toán gì?
Giải quyết bài toán tra cứu lịch sử phức tạp mà các LLM thông thường gặp khó khăn do thiếu dữ liệu chuyên sâu hoặc không có khả năng lập luận đa bước. Nó giúp người dùng tóm tắt diễn biến theo dòng thời gian và đối chiếu các sự kiện/số liệu một cách chính xác dựa trên nguồn tài liệu tin cậy.

#### 2. Ai là người dùng chính?
- Sinh viên và học sinh đang nghiên cứu lịch sử.
- Các nhà nghiên cứu, biên tập viên nội dung lịch sử cần kiểm chứng thông tin nhanh.
- Những người yêu thích lịch sử muốn tìm kiếm thông tin theo cách tương tác hiện đại.

#### 3. Vì sao chủ đề này phù hợp để phân tích deployment và cost?
- **Deployment:** Hệ thống sử dụng kiến trúc ReAct với nhiều bước (steps), việc quản lý độ trễ (latency) và lỗi trong chuỗi lập luận là thách thức lớn khi đưa ra thị trường.
- **Cost:** Agent gọi nhiều công cụ và tiêu tốn nhiều token hơn chatbot thông thường. Việc sử dụng Gemini Flash và tối ưu hóa số lượng sub-tasks là bài toán kinh tế then chốt để duy trì dịch vụ.
- **Monitoring:** Nhóm đã xây dựng sẵn hệ thống logging JSON và telemetry, là nền tảng hoàn hảo để phân tích các bài toán production thực tế.
