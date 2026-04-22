# Worksheet 2 - Cost Anatomy Lab

## 1. Ước lượng user/ngày, request/ngày, peak traffic

Nếu nói về một MVP thật sự, các số trước đó hơi đẹp và có thể chưa đủ thực tế. Một bộ giả định hợp lý hơn là:

- 200-500 user/ngày cho giai đoạn đầu
- 3-6 request/user/ngày
- khoảng 600-3,000 request/ngày

Ví dụ chọn mốc giữa để tính nhanh:

- 300 user/ngày
- 4 request/user/ngày
- 1,200 request/ngày

Peak traffic thường không đều. Nếu 30% request rơi vào 3 giờ cao điểm, thì:

- 360 request trong 3 giờ peak
- khoảng 120 request/giờ
- khoảng 2 request/phút

Con số này thực tế hơn cho một project sinh viên, demo MVP, hoặc sản phẩm nhỏ mới ra mắt. Nếu là app có login, history, RAG, hoặc nhiều lần retry, request/user/ngày có thể tăng lên 6-10.

## 2. Ước lượng input/output tokens nếu dùng LLM API

Một request AI thực tế thường không chỉ là prompt ngắn. Với chat/RAG nhẹ, một giả định hợp lý là:

- input: 500-1,500 tokens
- output: 150-400 tokens

Ví dụ chọn mốc giữa:

- input: 800 tokens
- output: 250 tokens
- tổng: 1,050 tokens/request

Với 1,200 request/ngày:

- input tokens/ngày = 800 x 1,200 = 960,000
- output tokens/ngày = 250 x 1,200 = 300,000
- tổng tokens/ngày = 1,260,000

Nếu có chat history dài hoặc context RAG lớn, input có thể vọt lên 2,000-4,000 tokens/request rất nhanh. Phần này là nơi nhiều team ước lượng quá lạc quan.

## 3. Các lớp cost cần tính

### Token cost

Đây thường là chi phí lớn nhất nếu hệ thống gọi LLM nhiều. Cost phụ thuộc vào:

- số request
- độ dài input
- độ dài output
- model sử dụng

### Compute cost

Bao gồm:

- backend API server
- queue worker
- embedding/retrieval pipeline
- xử lý file, OCR, preprocessing

### Storage cost

Gồm:

- database
- object storage cho file, ảnh, log
- vector database nếu dùng RAG

### Human review cost

Nếu có bước duyệt nội dung, kiểm tra chất lượng, hoặc moderation thủ công, đây là chi phí dễ bị bỏ sót nhưng có thể tăng rất nhanh khi scale.

### Logging/monitoring cost

Bao gồm:

- observability
- tracing
- error logging
- analytics

### Maintenance cost

Gồm:

- vận hành hệ thống
- sửa bug
- cập nhật prompt/model
- xử lý incident

## 4. Tính sơ bộ cost ở mức MVP

Để ra số tiền rõ hơn, dùng mốc 1,200 request/ngày và 1,050 tokens/request:

- 1,260,000 tokens/ngày
- 37,800,000 tokens/tháng

### Token cost theo một số model

Lấy giá tham chiếu từ pricing công khai của OpenAI:

- `gpt-5.4`: input $2.50 / 1M tokens, output $15.00 / 1M tokens
- `gpt-5.4-mini`: input $0.75 / 1M tokens, output $4.50 / 1M tokens
- `gpt-5.4-nano`: input $0.20 / 1M tokens, output $1.25 / 1M tokens
- `gpt-4o-mini`: input $0.15 / 1M tokens, output $0.60 / 1M tokens

Với 960,000 input tokens và 300,000 output tokens mỗi ngày:

- `gpt-5.4`: khoảng $4.20/ngày, tương đương khoảng $126/tháng
- `gpt-5.4-mini`: khoảng $1.03/ngày, tương đương khoảng $30.8/tháng
- `gpt-5.4-nano`: khoảng $0.39/ngày, tương đương khoảng $11.8/tháng
- `gpt-4o-mini`: khoảng $0.22/ngày, tương đương khoảng $6.5/tháng

Nhận xét:

- model càng mạnh thì output cost tăng rất nhanh
- nếu app có output dài, phần output thường đắt hơn input
- nếu dùng model rẻ cho đa số request và chỉ escalates một phần nhỏ sang model mạnh, chi phí có thể giảm mạnh

### Infra cost

MVP thường có thể chạy với:

- 1-2 app server nhỏ
- 1 database
- 1 storage bucket
- 1 logging/monitoring service

Phần infra có thể thấp hơn token cost, nhưng nếu dùng vector DB, Redis, queue, hoặc logging service trả tiền theo volume thì cũng không còn quá nhỏ.

### Infra cost

MVP thường có thể chạy với:

- 1-2 app server nhỏ
- 1 database
- 1 storage bucket
- 1 logging/monitoring service

Phần infra có thể thấp hơn token cost khá nhiều, trừ khi có traffic lớn hoặc xử lý file nặng.

### Human review cost

Nếu chỉ 1-5% request cần review:

- 12 đến 60 case/ngày

Với 1,200 request/ngày, đây là mức hợp lý hơn cho MVP. Tuy nhiên nếu chất lượng output chưa ổn, con số này có thể tăng rất nhanh và trở thành chi phí ẩn đáng kể.

### Kết luận sơ bộ

Ở mức MVP, cost thường không nằm ở server truyền thống mà nằm ở:

1. LLM token usage
2. Human review
3. Logging/monitoring nếu bật quá nhiều

## 5. Khi user tăng 5x hoặc 10x, phần nào tăng mạnh nhất

### Tăng 5x

Nếu từ 1,000 user/ngày lên 5,000 user/ngày:

- request tăng gần tuyến tính
- token cost tăng gần tuyến tính
- storage và logging tăng theo dữ liệu phát sinh
- human review có thể tăng nhanh hơn tuyến tính nếu chất lượng giảm và cần kiểm tra nhiều hơn

### Tăng 10x

Nếu lên 10,000 user/ngày:

- LLM token cost gần như chắc chắn là chi phí lớn nhất
- hệ thống retrieval, database, logging bắt đầu cần tối ưu
- nếu có queue hoặc rate limit, peak traffic có thể gây nghẽn trước khi tổng daily traffic trở thành vấn đề

### Phần tăng mạnh nhất

Thường là:

1. Token cost
2. Human review cost
3. Compute cho inference phụ trợ, retrieval, background jobs

## Trả lời 3 câu hỏi chính

### Cost driver lớn nhất của hệ thống là gì?

Thường là **LLM token cost**. Nếu có human-in-the-loop, chi phí review thủ công có thể trở thành cost driver thứ hai.

### Hidden cost nào dễ bị quên nhất?

Các hidden cost dễ bị quên nhất là:

- logging và monitoring
- human review
- retry do lỗi API hoặc timeout
- prompt iteration và maintenance
- storage của logs, traces, files

### Đội có chỗ nào đang ước lượng quá lạc quan không?

Có thể lạc quan ở các điểm sau:

- ước lượng output tokens quá thấp
- bỏ qua chat history hoặc context dài
- giả định mọi request đều giống nhau
- quên chi phí review thủ công
- quên chi phí retry, timeout, và logging

## Kết luận

Với một AI system, đừng chỉ nhìn vào giá API/token. Cần bóc tách theo toàn bộ pipeline:

- traffic
- token usage
- compute
- storage
- human review
- logging/maintenance

Ở MVP, token cost thường là khoản lớn nhất, nhưng khi chất lượng chưa ổn hoặc traffic tăng, human review và vận hành có thể trở thành chi phí rất đáng kể.
