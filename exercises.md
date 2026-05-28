# Ngày 1 — Bài Tập & Phản Ánh
## Nền Tảng LLM API | Phiếu Thực Hành

**Thời lượng:** 1:30 giờ  
**Cấu trúc:** Lập trình cốt lõi (60 phút) → Bài tập mở rộng (30 phút)

---

## Phần 1 — Lập Trình Cốt Lõi (0:00–1:00)

Chạy các ví dụ trong Google Colab tại: https://colab.research.google.com/drive/172zCiXpLr1FEXMRCAbmZoqTrKiSkUERm?usp=sharing

Triển khai tất cả TODO trong `template.py`. Chạy `pytest tests/` để kiểm tra tiến độ.

**Điểm kiểm tra:** Sau khi hoàn thành 4 nhiệm vụ, chạy:
```bash
python template.py
```
Bạn sẽ thấy output so sánh phản hồi của GPT-4o và GPT-4o-mini.

---

## Phần 2 — Bài Tập Mở Rộng (1:00–1:30)

### Bài tập 2.1 — Độ Nhạy Của Temperature
Gọi `call_openai` với các giá trị temperature 0.0, 0.5, 1.0 và 1.5 sử dụng prompt **"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature càng thấp (0.0), câu trả lời thường ổn định, ngắn gọn và ít thay đổi giữa các lần gọi. Khi temperature tăng lên (0.5 → 1.5), phản hồi trở nên sáng tạo hơn, đa dạng hơn nhưng cũng có thể dài dòng hoặc đôi khi kém chính xác hơn. Temperature cao giúp tăng tính ngẫu nhiên và phong cách diễn đạt phong phú hơn.

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Nhạy Của Temperature

Bạn nhận thấy quy luật gì qua bốn phản hồi?

Khi temperature càng thấp (0.0), câu trả lời thường ổn định, ngắn gọn và ít thay đổi giữa các lần gọi. Khi temperature tăng lên (0.5 → 1.5), phản hồi trở nên sáng tạo hơn, đa dạng hơn nhưng cũng có thể dài dòng hoặc đôi khi kém chính xác hơn. Temperature cao giúp tăng tính ngẫu nhiên và phong cách diễn đạt phong phú hơn.

Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?

Tôi sẽ đặt temperature khoảng 0.2–0.5 cho chatbot hỗ trợ khách hàng. Mức này giúp phản hồi ổn định, chính xác và nhất quán, đồng thời vẫn giữ được sự tự nhiên trong cách giao tiếp với người dùng.

---

### Bài tập 2.2 — Đánh Đổi Chi Phí
Xem xét kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người thực hiện 3 lần gọi API, mỗi lần trung bình ~350 token.

**Ước tính xem GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này:**
> Workload mỗi ngày:

10.000 người dùng
Mỗi người 3 lần gọi API
Mỗi lần ~350 token

Tổng token/ngày ≈ 10.000 × 3 × 350 = 10.500.000 token.

Theo mức giá phổ biến, GPT-4o thường đắt hơn GPT-4o-mini khoảng 5–10 lần tùy loại token input/output. Với workload lớn như trên, chi phí chênh lệch mỗi tháng có thể lên tới hàng nghìn USD.

**Mô tả một trường hợp mà chi phí cao hơn của GPT-4o là xứng đáng, và một trường hợp GPT-4o-mini là lựa chọn tốt hơn:**
> GPT-4o phù hợp trong các tác vụ yêu cầu chất lượng cao như phân tích tài liệu phức tạp, trợ lý AI chuyên nghiệp hoặc xử lý đa phương thức (hình ảnh, âm thanh) với độ chính xác cao.

GPT-4o-mini phù hợp hơn cho chatbot FAQ, hỗ trợ khách hàng cơ bản hoặc ứng dụng có lượng request lớn cần tối ưu chi phí nhưng vẫn đảm bảo tốc độ và chất lượng đủ tốt.

---

### Bài tập 2.3 — Trải Nghiệm Người Dùng với Streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng cần phản hồi theo thời gian thực như chatbot, trợ lý AI hoặc công cụ hỗ trợ lập trình, vì người dùng có thể thấy nội dung xuất hiện dần thay vì phải chờ toàn bộ kết quả hoàn tất. Điều này giúp trải nghiệm nhanh hơn và tự nhiên hơn, đặc biệt với câu trả lời dài. Ngược lại, non-streaming phù hợp hơn trong các trường hợp cần xử lý hoàn chỉnh trước khi hiển thị, ví dụ như trả về dữ liệu JSON, báo cáo tổng hợp hoặc các tác vụ yêu cầu tính toàn vẹn của kết quả trước khi hiển thị cho người dùng.


## Danh Sách Kiểm Tra Nộp Bài
- [ ] Tất cả tests pass: `pytest tests/ -v`
- [ ] `call_openai` đã triển khai và kiểm thử
- [ ] `call_openai_mini` đã triển khai và kiểm thử
- [ ] `compare_models` đã triển khai và kiểm thử
- [ ] `streaming_chatbot` đã triển khai và kiểm thử
- [ ] `retry_with_backoff` đã triển khai và kiểm thử
- [ ] `batch_compare` đã triển khai và kiểm thử
- [ ] `format_comparison_table` đã triển khai và kiểm thử
- [ ] `exercises.md` đã điền đầy đủ
- [ ] Sao chép bài làm vào folder `solution` và đặt tên theo quy định 
