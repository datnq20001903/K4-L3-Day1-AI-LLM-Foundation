# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi tăng tempature thì AI sẽ sáng tạo hơn, ở mức thấp 0.0 - 0.5, AI sẽ đưa ra câu trả lời chính xác, còn 1.5 thì sẽ sáng tạo nhưng giảm độ tin cậy.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature 0.5, như vậy chatbot sẽ trả lời ổn định, chính xác

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Ước tính GPT-4o đắt hơn khoảng 16,7 lần so với GPT-4o-mini. GPT-4o xứng đáng với chi phí trong trường hợp dứ án quy mô 


## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với cùng câu hỏi "Giải thích blockchain là gì?", hai system prompt sẽ tạo ra hai phản hồi khác biệt rõ rệt. Phiên bản "giáo viên tiểu học" thường ngắn gọn hơn, dùng ví dụ đời thường (như so sánh blockchain với "cuốn sổ ghi chép chung mà ai cũng thấy được, không ai xóa được"), tránh thuật ngữ chuyên môn, câu văn ngắn và giọng điệu thân thiện, gần gũi. Ngược lại, phiên bản "chuyên gia tài chính" sẽ dài hơn, dùng thuật ngữ kỹ thuật (hash, consensus, distributed ledger, proof-of-work...), cấu trúc lập luận chặt chẽ hơn và có thể đề cập đến ứng dụng thực tế trong tài chính/ngân hàng.

Điều này cho thấy system prompt không thay đổi *kiến thức* mà model có, nhưng ảnh hưởng mạnh đến *cách trình bày* — độ dài, mức độ trừu tượng, lựa chọn từ vựng và đối tượng giả định mà model "nghĩ" mình đang nói chuyện cùng. Nói cách khác, system prompt định hình vai trò (persona) và ngữ cảnh giao tiếp, khiến cùng một nội dung được "dịch" sang các mức độ phức tạp và phong cách khác nhau, dù thông tin cốt lõi (blockchain là gì) về cơ bản vẫn đúng và nhất quán.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> 2 con số chênh nhau khoảng 20%. Với cùng một ý, câu tiếng Việt thường tốn nhiều token hơn câu tiếng Anh, vì dấu thanh bị tách thành nhiều token.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng có tương tác trực tiếp với người dùng và cần cảm giác phản hồi tức thì — điển hình là chatbot, trợ lý ảo, hay các giao diện hội thoại nơi người dùng đang chờ xem câu trả lời hiện ra.Ngược lại, non-streaming phù hợp hơn khi ứng dụng cần xử lý toàn bộ phản hồi trước khi dùng nó — ví dụ như khi cần parse kết quả dưới dạng JSON có cấu trúc, khi phản hồi được dùng làm input cho một bước xử lý tiếp theo trong pipeline tự động (không có người xem trực tiếp), khi cần tính toán chi phí/token hoặc log lại kết quả hoàn chỉnh trước khi trả về, hoặc trong các tác vụ chạy nền, batch processing, nơi tốc độ hiển thị theo thời gian thực không mang lại giá trị nào.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> So với delay cố định, exponential backoff có lợi thế là giãn dần thời gian chờ giữa các lần retry (ví dụ 1s, 2s, 4s, 8s...), giúp giảm áp lực lên server đang quá tải thay vì tiếp tục dồn dập request vào đúng lúc hệ thống cần thời gian để phục hồi. Nếu request đầu thất bại vì server quá tải, việc chờ lâu hơn ở các lần sau cho hệ thống cơ hội "thở" và xử lý hàng đợi hiện có, thay vì cộng thêm tải mới liên tục.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona chọn: Trợ lý tư vấn học tập cho sinh viên năm nhất ngành CNTT.System prompt:"Bạn là một trợ lý học tập ngành công nghệ thông tin , nhiệm vụ của bạn là giải thích các khái niệm lập trình và khoa học máy tính một cách ngắn gọn rõ ràng, dễ hiểu bằng tiếng Việt." Trả lời ngắn gọn vì nó sẽ giúp model nêu ý chính, tránh lan man. Bằng tiếng Việt để AI không tự chuyển ngôn ngữ.


### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế là hiện tại không có bộ nhớ, mỗi lần chat là lại như mới.Đề xuất cải thiện: Xây dựng một "hồ sơ học tập" (learning profile) dạng tóm tắt ngắn, lưu trữ ngoài context window (ví dụ trong database hoặc file JSON gắn với user_id), ghi lại: các chủ đề đã học, các lỗi sai thường gặp, và mức độ hiểu hiện tại (ví dụ: "đã nắm vững loop và list, còn yếu về đệ quy").

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
