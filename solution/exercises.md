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
>Dù ở mức temperature nào, mô hình vẫn chọn cùng một sự thật (hang Sơn Đoòng), nhưng cách diễn đạt và từ vựng thay đổi rõ rệt khi temperature tăng lên. Ở các mức thấp (0.0 và 0.5), văn phong thiên về liệt kê số liệu và thông tin tiêu chuẩn; trong khi ở mức cao (1.5), mô hình sử dụng ngôn từ linh hoạt, mang tính hình ảnh bay bổng hơn (ví dụ: ví von với "tòa nhà chọc trời 40 tầng"). Điều này cho thấy temperature cao làm tăng tính đa dạng và sáng tạo trong từ vựng, nhưng cấu trúc lõi của câu trả lời vẫn được giữ nguyên.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
>Đối với chatbot hỗ trợ khách hàng, mức temperature lý tưởng nên được đặt ở khoảng thấp (từ 0.0 đến 0.3). Lý do là chatbot hỗ trợ khách hàng cần ưu tiên tính chính xác, nhất quán và đáng tin cậy. Khi khách hàng hỏi về chính sách, giá cả hay các bước khắc phục sự cố, hệ thống cần đưa ra những thông tin chuẩn xác và giống nhau cho mọi khách hàng thay vì sáng tạo ra các câu trả lời mới lạ. Mức temperature thấp sẽ giúp mô hình bám sát vào tài liệu hướng dẫn và giảm thiểu tối đa rủi ro "ảo giác" (hallucination) — tức là việc mô hình tự bịa ra thông tin hoặc chính sách không có thật.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
>Dựa trên bảng giá hiện tại của OpenAI:
GPT-4o: Đầu vào: $2.50 / 1M token | Đầu ra: $10.00 / 1M token
GPT-4o-mini: Đầu vào: $0.150 / 1M token | Đầu ra: $0.600 / 1M token
Ước tính chênh lệch chi phí: Chi phí của GPT-4o cao hơn GPT-4o-mini khoảng 16,67 lần (ở cả đầu vào và đầu ra: 10 / 0.6 ≈ 16.67). Đối với kịch bản có lượng người dùng lớn (10.000 users × 3 lượt = 30.000 lượt gọi/ngày), sự chênh lệch 16.67 lần này sẽ tạo ra một khoảng cách khổng lồ về mặt ngân sách (hàng nghìn USD mỗi tháng).
Trường hợp sử dụng:
Nên dùng GPT-4o (Xứng đáng với chi phí): Các tác vụ đòi hỏi khả năng tư duy logic và suy luận phức tạp, phân tích dữ liệu chuyên sâu, viết code phức tạp, hoặc khi cần đọc hiểu hình ảnh/tài liệu khó (ví dụ: trợ lý phân tích tài chính, chatbot tư vấn pháp lý).
Nên dùng GPT-4o-mini: Các tác vụ có khối lượng lớn (high-volume) nhưng yêu cầu suy luận cơ bản như: tóm tắt văn bản, phân loại ý định khách hàng (intent classification), dịch thuật cơ bản, hoặc chatbot trả lời các câu hỏi FAQ đơn giản dựa trên tài liệu có sẵn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
>Sự khác biệt giữa hai phản hồi là rất rõ rệt. Ở vai "giáo viên tiểu học", câu trả lời rất ngắn gọn, dùng từ vựng gần gũi, đời thường và sử dụng phép ẩn dụ trực quan (ví dụ: ví blockchain như "một cuốn sổ ghi chép chung", giao dịch như "trao đổi thẻ bài"). Ngược lại, ở vai "chuyên gia tài chính", câu trả lời dài hơn, có cấu trúc chặt chẽ (phân chia gạch đầu dòng rõ ràng) và sử dụng nhiều thuật ngữ chuyên môn sâu (như "sổ cái phân tán", "phi tập trung", "bất biến", "thuật toán đồng thuận Proof of Work"). Điều này cho thấy System prompt có sức mạnh chi phối hoàn toàn bối cảnh, mức độ chuyên môn, giọng văn và đối tượng mục tiêu của mô hình, giúp định hướng cách AI phản hồi ngay từ trước khi nó xử lý câu hỏi thật của người dùng.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
>Đoạn văn ví dụ (~134 từ): "Việt Nam là một quốc gia nằm ở bán đảo Đông Dương thuộc khu vực Đông Nam Á. Đất nước này có hình chữ S, phía bắc giáp Trung Quốc, phía tây giáp Lào và Campuchia, phía đông và phía nam giáp Biển Đông. Với lịch sử hàng ngàn năm văn hiến, Việt Nam tự hào về một nền văn hóa đa dạng, phong phú với 54 dân tộc anh em cùng chung sống. Cảnh quan thiên nhiên của Việt Nam vô cùng tươi đẹp, từ những dãy núi hùng vĩ ở phía Bắc, những bãi biển cát trắng trải dài ở miền Trung..."
Số từ thực tế: 134 từ
Ước tính theo công thức (số từ / 0.75): ~178 token
Số token đếm bằng tiktoken (GPT-4o): 183 token
Độ chênh lệch: Thực tế cao hơn ước lượng khoảng ~2.8%.
Vì sao tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài?
Đặc điểm ngôn ngữ đơn âm tiết: Trong tiếng Việt, các âm tiết (tiếng) được viết tách rời bằng dấu cách (ví dụ: "quốc gia" là 2 từ đơn), trong khi tiếng Anh thường gộp thành 1 từ (ví dụ: "nation"). Do đó, cùng một lượng thông tin, tiếng Việt có số lượng "từ" (cách nhau bởi dấu cách) nhiều hơn tiếng Anh.
Hạn chế của bộ mã hóa (Tokenizer): Tokenizer của các mô hình LLM chủ yếu được huấn luyện trên kho dữ liệu khổng lồ bằng tiếng Anh. Một từ tiếng Anh phổ biến có thể được mã hóa thành 1 token, nhưng một từ tiếng Việt có dấu (đặc biệt là các từ ít phổ biến) thường bị chẻ nhỏ thành 2-3 token.
---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
>Streaming quan trọng nhất trong các ứng dụng tương tác trực tiếp với con người (như chatbot, trợ lý ảo, công cụ viết bài dài), vì nó giúp giảm thiểu "thời gian chờ đến token đầu tiên" (Time To First Token), tạo cảm giác hệ thống phản hồi ngay lập tức và giữ chân người dùng trong lúc chờ sinh xong toàn bộ văn bản. Ngược lại, non-streaming phù hợp hơn cho các tác vụ xử lý ngầm (batch processing), trích xuất dữ liệu có cấu trúc (ví dụ: yêu cầu trả về JSON để code đọc), hoặc các hệ thống tự động hóa mà ở đó toàn bộ kết quả cần được tính toán hoặc xác thực xong xuôi trước khi chuyển sang bước tiếp theo (không có người dùng ngồi xem trực tiếp).

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
>Khi API bị quá tải, exponential backoff giúp giảm dần áp lực lên server bằng cách giãn cách thời gian giữa các lần thử lại (ví dụ: chờ 1s, rồi 2s, rồi 4s...). Việc này cho server thêm không gian và thời gian để phục hồi. Nếu hàng nghìn client cùng thử lại với một delay cố định giống nhau (ví dụ: tất cả cùng đếm ngược đúng 1 giây rồi gọi lại), nó sẽ tạo ra hiện tượng "đàn bò hoảng loạn" (thundering herd problem). Toàn bộ lượng request khổng lồ sẽ đồng loạt dội vào máy chủ cùng một phần nghìn giây, khiến server vừa mới ngoi lên lại tiếp tục sập hoặc bị quá tải vĩnh viễn.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona: Trợ lý học tập chuyên lập trình cho người mới bắt đầu. System Prompt: "Bạn là trợ lý học tập lập trình thân thiện. Luôn trả lời ngắn gọn bằng tiếng Việt, cung cấp code mẫu kèm bình luận chi tiết, và dùng phép ẩn dụ đời thường để giải thích các khái niệm khó." 
Giải thích từ ngữ quan trọng:
"bằng tiếng Việt": Bắt buộc mô hình giữ ngôn ngữ nhất quán, tránh tình trạng tự động chuyển sang tiếng Anh khi gặp các từ khóa lập trình (vì tài liệu lập trình chủ yếu bằng tiếng Anh).
"ngắn gọn": Đảm bảo mô hình đi thẳng vào vấn đề thay vì viết các đoạn lý thuyết dài dòng gây quá tải thông tin cho người mới bắt đầu.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất: Trợ lý hiện tại sử dụng cơ chế "cắt cụt lịch sử" (chỉ giữ lại 3 lượt gần nhất). Điều này khiến nó mắc bệnh "mất trí nhớ ngắn hạn" — nó sẽ quên hoàn toàn bối cảnh tổng thể của cuộc trò chuyện, tên người dùng, hay mục tiêu ban đầu ngay khi cuộc hội thoại vượt quá 3 lượt.
Đề xuất cải thiện: Triển khai "Tóm tắt lịch sử cuốn chiếu" (Sliding Window Summarization). Cách triển khai: Thay vì xóa hẳn các tin nhắn cũ hơn 3 lượt, mỗi khi lịch sử đạt đến ngưỡng, ta sẽ gọi API một lần (bằng một model giá rẻ như GPT-4o-mini) để yêu cầu mô hình tóm tắt lại toàn bộ nội dung các tin nhắn cũ đó thành 1-2 câu ngắn gọn. Sau đó, ta nối phần "tóm tắt bối cảnh" này vào đầu mảng history mới. Nhờ vậy, trợ lý vừa tiết kiệm được token ở mỗi lượt gọi, vừa duy trì được bộ nhớ dài hạn.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
