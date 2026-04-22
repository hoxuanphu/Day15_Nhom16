# AI Chatbot Lịch Sử – Phân tích vấn đề & cách xử lý

## Mục tiêu
- Hoạt động ổn định khi tải cao.
- Giảm thiểu lỗi từ provider.
- Đảm bảo độ chính xác thông tin.
- Duy trì trải nghiệm người dùng tốt.

---

## I. Tổng quan hệ thống

### Các loại request
- Hỏi đáp cơ bản (fact-based).
- Giải thích chuyên sâu.
- So sánh sự kiện/nhân vật.
- Timeline lịch sử.
- Trích nguồn tham khảo.
- Hội thoại nhiều lượt.

### Đặc điểm hệ thống
- Phụ thuộc LLM + RAG.
- Yêu cầu độ chính xác cao.
- Không phải mọi request đều cần real-time.

---

## II. Các vấn đề & cách xử lý

### 1. Traffic tăng đột biến (High Load)
- **Tình huống**: Người dùng tăng đột ngột.
- **Tác động**: Chậm, timeout, mất request.

#### Cách xử lý ngắn hạn
- **Rate Limiting**: Giới hạn 10 req/phút/user, vượt quá trả 429.
- **Queue**: Đưa yêu cầu vào hàng đợi, worker xử lý sau.
- **Giảm chất lượng có kiểm soát**: Dùng model nhỏ, trả lời ngắn gọn.
- **Caching**: Lưu câu hỏi phổ biến (VD: "Hitler là ai?").

#### Giải pháp dài hạn
- **Auto Scaling**: Tự động tăng/giảm server theo tải.
- **Model Routing**: Câu đơn giản → model nhỏ, câu phức tạp → model lớn.

#### Metrics & Fallback
- Metrics: QPS, p95/p99 latency, error rate, queue length.
- Fallback: Thông báo bận, chuyển xử lý async.

---

### 2. Provider Timeout / API lỗi
- **Tình huống**: LLM API không phản hồi.
- **Tác động**: Chat treo.

#### Cách xử lý ngắn hạn
- **Retry với Exponential Backoff**: 1s → 2s → 4s.
- **Timeout control**: Hủy yêu cầu sau 10s.
- **Circuit Breaker**: Ngắt tạm thời nếu lỗi liên tục.

#### Giải pháp dài hạn
- **Multi-provider**: Chính + backup.
- **Local model fallback**: Dùng model nội bộ khi API chết.

#### Metrics & Fallback
- Metrics: Success rate, timeout rate, retry count.
- Fallback: Trả lời đơn giản hoặc thông báo lỗi.

---

### 3. Response chậm (High Latency)
- **Tình huống**: Query phức tạp hoặc RAG chậm.
- **Tác động**: Người dùng chờ lâu.

#### Cách xử lý ngắn hạn
- **Streaming response**: Trả từng phần thay vì đợi hoàn tất.
- **Hiển thị trạng thái**: "Đang xử lý...".
- **Giảm context**: Lấy ít tài liệu hơn từ RAG.

#### Giải pháp dài hạn
- **Tối ưu RAG**: Dùng Vector DB, chunk kích thước hợp lý.
- **Tối ưu prompt**: Cắt giảm token dư thừa.

#### Metrics & Fallback
- Metrics: Latency tổng, retrieval time, token usage.
- Fallback: Trả lời dạng bullet ngắn.

---

### 4. Hallucination (AI trả lời sai)
- **Tình huống**: Thông tin lịch sử bịa đặt.
- **Tác động**: Mất uy tín hệ thống.

#### Cách xử lý ngắn hạn
- **Confidence threshold**: Không trả lời nếu không chắc chắn.
- **Hiển thị nguồn**: Luôn kèm tài liệu tham khảo.

#### Giải pháp dài hạn
- **RAG pipeline chặt chẽ**: Chỉ trả lời dựa trên tài liệu truy xuất.
- **Fact-check layer**: Model thứ hai kiểm tra tính xác thực.

#### Metrics & Fallback
- Metrics: Tỉ lệ hallucination, phản hồi người dùng.
- Fallback: "Không chắc chắn", đề xuất nguồn uy tín.

---

### 5. Context bị mất
- **Tình huống**: Model quên nội dung hội thoại trước.
- **Cách xử lý**:
  - Lưu lịch sử chat vào DB/bộ nhớ.
  - Tóm tắt context khi vượt quá giới hạn token.
  - Entity tracking để theo dõi nhân vật/sự kiện chính.
- **Fallback**: Chủ động hỏi lại người dùng.

---

### 6. Nội dung nhạy cảm
- **Cách xử lý**:
  - Moderation input (từ khóa hoặc model phân loại).
  - Giữ giọng điệu trung lập, khách quan.
  - Từ chối lịch sự nếu câu hỏi mang tính khiêu khích.

---

### 7. Spam / Abuse
- **Cách xử lý**:
  - Rate limit theo IP.
  - CAPTCHA khi có dấu hiệu nghi ngờ.
  - Phát hiện bot qua phân tích hành vi.

---

### 8. Cost tăng cao
- **Cách xử lý**:
  - Cache câu hỏi phổ biến.
  - Giới hạn token input/output mỗi request.
  - Dùng model nhỏ cho đa số lưu lượng, model lớn cho câu hỏi phức tạp.

---

## III. Phân loại request
- **Real-time**: Câu hỏi ngắn, fact đơn giản → ưu tiên xử lý nhanh.
- **Async**: Phân tích dài, timeline → xử lý bất đồng bộ, trả kết quả sau.

---

## IV. Kiến trúc đề xuất
- API Gateway → Queue → LLM Service / RAG Service → Cache → Monitoring.
- Tách biệt nhận request và xử lý, dễ mở rộng và giám sát.

---

## V. Tổng kết
- Không sập khi đông người.
- Không phụ thuộc 1 provider.
- Không trả lời sai.
- Không để người dùng chờ lâu.

### Nguyên tắc quan trọng
- **Luôn có fallback – vì hệ thống thực tế chắc chắn sẽ lỗi.**