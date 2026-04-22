# Bản đề xuất lộ trình phát triển (Track Recommendation)
## Dự án: History Chatbot - Nhóm 16

### 1. Lựa chọn Track: AI Application (Advanced RAG & Agenty)

Sau khi đánh giá năng lực hiện tại và mục tiêu phát triển của dự án History Chatbot, nhóm quyết định tập trung vào Track **AI Application (Advanced RAG & Agenty)** cho Phase 2.

**Lý do lựa chọn:**
- **Tính đặc thù của dữ liệu:** Lịch sử là mảng kiến thức rộng, đòi hỏi sự chính xác tuyệt đối về mốc thời gian và sự kiện. RAG cơ bản không đủ để xử lý các tài liệu phức tạp và đa chiều.
- **Yêu cầu về tương tác:** Chatbot cần khả năng suy luận (Reasoning) để trả lời các câu hỏi so sánh hoặc phân tích nguyên nhân - kết quả, điều mà hướng tiếp cận "Agenty" (sử dụng Agent + Tool) sẽ giải quyết tốt hơn chatbot thông thường.
- **Tính ứng dụng cao:** Track này giúp nhóm làm chủ toàn bộ vòng đời của một ứng dụng AI từ thực tế đến tay người dùng.

---

### 2. Kế hoạch bù đắp kỹ năng (Skills Gap Improvement)

Để thành công ở Track này, nhóm xác định hai trụ cột kỹ thuật cần phải nâng cấp ngay lập tức:

#### A. Advanced Knowledge Retrieval: GraphRAG
Vì dự án đã có Hybrid Search và Rerank, nhóm sẽ tiến cấp lên **GraphRAG** để giải quyết bài toán cốt lõi của lịch sử - **Sự kết nối giữa các thực thể**:
- **Knowledge Graph Integration:** Xây dựng đồ thị tri thức (sử dụng Neo4j hoặc NetworkX) để kết nối Nhân vật - Sự kiện - Địa danh - Thời gian. Thay vì chỉ tìm các đoạn văn bản tương đương, hệ thống sẽ có khả năng truy vấn theo quan hệ (VD: "Các sự kiện diễn ra cùng thời điểm với Chiến dịch Điện Biên Phủ").
- **Entity Extraction & Linkage:** Tự động trích xuất thực thể từ tài liệu lịch sử và thiết lập liên kết giữa chúng để tạo nên mạng lưới tri thức sâu dữ liệu.

#### B. Ngôn ngữ & Suy luận: Multi-Agent Orchestration & CRAG
Thay vì một Agent duy nhất xử lý mọi việc, nhóm sẽ chuyển sang mô hình chuyên biệt hóa:
- **Multi-Agent Workflow:** Chia hệ thống thành các Specialist Agents:
    - *Historian Agent:* Chuyên kiểm chứng sự kiện và mốc thời gian.
    - *Retrieval Agent:* Chuyên tối ưu hóa việc tìm kiếm từ Graph và Vector DB.
    - *Tutor Agent:* Chuyên cá nhân hóa câu trả lời cho học sinh.
- **Corrective RAG (CRAG):** Tích hợp bước tự đánh giá (Self-correction). Nếu kết quả tìm kiếm không đủ tin cậy, Agent sẽ tự động kích hoạt công cụ tìm kiếm bên ngoài (Search Tool) hoặc điều chỉnh lại chiến thuật truy vấn thay vì trả lời sai.

---

### 3. Kế hoạch hành động chi tiết (Next Steps)

Nhóm sẽ thực hiện lộ trình Phase 2 trong vòng 4-6 tuần tiếp theo:

| Tuần | Mục tiêu chính | Công việc cụ thể |
| :--- | :--- | :--- |
| **Tuần 1** | **Xây dựng Graph Schema** | Thiết kế cấu trúc đồ thị tri thức lịch sử (Node, Relationship types). Trích xuất thực thể từ dữ liệu hiện có. |
| **Tuần 2** | **Triển khai GraphRAG** | Tích hợp công cụ truy vấn đồ thị vào pipeline RAG hiện tại. Thực hiện thử nghiệm tìm kiếm theo mối quan hệ. |
| **Tuần 3** | **Thiết kế Multi-Agent** | Xây dựng quy trình phối hợp giữa các Agent bằng LangGraph hoặc CrewAI. Định nghĩa "Hand-off" logic giữa các bộ phận. |
| **Tuần 4** | **Vận hành LLMOps** | Cài đặt Langfuse để giám sát sự tương tác giữa các Agent. Đánh giá độ trễ của quy trình Multi-Agent. |
| **Tuần 5** | **Optimization & CRAG** | Tinh chỉnh bước Self-correction để giảm Hallucination. Test các trường hợp câu hỏi lắt léo. |
| **Tuần 6** | **Product Delivery** | Đóng gói sản phẩm, hoàn thiện giao diện hỗ trợ hiển thị luồng suy luận của Agent (Reasoning visualization). |

---

**Kết luận:** Với lộ trình này, nhóm 16 tin rằng History Chatbot sẽ không chỉ là một công cụ tra cứu thông tin mà còn trở thành một "Trợ lý học thuật" thực sự, có khả năng tư duy và phản hồi chính xác, tin cậy.
