# Worksheet 3 – Cost Optimization Debate

**Hệ thống:** Sử Việt AI Agent

---

### 1. Ba chiến lược ưu tiên áp dụng

1. **Semantic Caching**  
2. **Model Routing**  
3. **Selective Inference / Phân tầng Request**

*(Lý do loại bỏ Prompt Compression: Domain lịch sử yêu cầu độ chính xác tuyệt đối về ngày tháng và tên riêng, nén prompt có thể làm sai lệch sự kiện).*

---

### 2. Chi tiết Chiến lược

#### Chiến lược 1: Semantic Caching
- **Cơ chế tiết kiệm:** Bỏ qua toàn bộ chu trình Planner → Executor → LLM Synthesis cho các câu hỏi tương tự.
- **Lợi ích:** Dữ liệu lịch sử 1965-1975 là bất biến, tần suất hỏi trùng lặp cao (VD: "Chiến dịch Hồ Chí Minh diễn ra khi nào?"). Hit rate cao giúp giảm mạnh latency và token cost.
- **Trade-off:** Cần duy trì Vector Database cho Cache, rủi ro trả lời sai nếu ngưỡng similarity quá thấp.
- **Thời điểm áp dụng:** **LÀM NGAY**. Đây là "quick win" tách biệt khỏi core logic.

#### Chiến lược 2: Model Routing
- **Cơ chế tiết kiệm:** Chọn model rẻ hơn cho các tác vụ không đòi hỏi suy luận sâu.
- **Cụ thể:**
  - **Planner (Phân rã câu hỏi):** Dùng model lớn (VD: GPT-5.4-mini).
  - **Executor (Tổng hợp dữ liệu đã retrieve):** Dùng model nhỏ/gọn (VD: Gemini Flash).
  - **Classifier (Phân loại câu hỏi ngoài lề):** Dùng model rất nhỏ hoặc rule-based.
- **Trade-off:** Tăng độ phức tạp pipeline.
- **Thời điểm áp dụng:** **LÀM NGAY**. Phù hợp tự nhiên với kiến trúc Planner-Executor sẵn có.

#### Chiến lược 3: Selective Inference / Phân tầng Request
- **Cơ chế tiết kiệm:** Không phải câu hỏi nào cũng cần đến Agent "hạng nặng".
- **Phân loại:**
  - **Simple QA:** "Trận Điện Biên Phủ trên không năm nào?" → Chỉ cần RAG đơn giản.
  - **Complex QA:** "So sánh thương vong giữa Mậu Thân 1968 và Điện Biên Phủ 1972" → Cần Planner-Executor đầy đủ.
  - **Out-of-Scope:** "Công thức nấu phở bò" → Từ chối ngay bằng classifier rẻ tiền.
- **Trade-off:** Cần đủ dữ liệu thực tế để huấn luyện bộ phân loại chính xác.
- **Thời điểm áp dụng:** **ĐỂ SAU**. Cần có log người dùng thật để đánh giá độ phức tạp trước khi triển khai.

---

### 3. Lộ trình thực hiện

| Ưu tiên | Chiến lược | Lý do |
| :---: | :--- | :--- |
| **1** | Semantic Caching | ROI cao nhất, không ảnh hưởng chất lượng câu trả lời. |
| **2** | Model Routing | Tận dụng tối đa kiến trúc hiện có, giảm chi phí mỗi lượt gọi. |
| **3** | Selective Inference | Sẽ phát triển sau khi có đủ dữ liệu production (Phase 2). |
