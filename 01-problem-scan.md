Phase 1
| # | Subsidiary | Lens                                          | Mô tả ngắn bài toán                                                                                                                                                                                                                                      |
| - | ---------- | --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 | Xanh SM    | Dispatch thủ công / bán rule-based            | Điều phối xe – khách dựa trên vị trí gần nhất + kinh nghiệm dispatcher, chưa tối ưu ETA/traffic theo thời gian thực → gây xe chạy vòng và tăng thời gian chờ. Ước tính thất thoát: **10–18% idle mileage**, **8–12% giảm hiệu suất fleet giờ cao điểm**. |
| 2 | Xanh SM    | Dự báo nhu cầu theo giờ/khu vực thủ công      | Ops team dùng dashboard + kinh nghiệm để điều xe theo “hot zone” thay vì forecast chuẩn hóa. Sai lệch cung–cầu gây **15–25% mismatch**, làm mất doanh thu giờ peak và tăng xe rỗng.                                                                      |
| 3 | Xanh SM    | Xử lý khiếu nại khách hàng thủ công           | CSKH phân loại complaint (trễ xe, thái độ, mất đồ) bằng đọc tay + nhập CRM → mất 3–8 phút/case. Với ~50k case/tháng → mất **2.500–6.000 giờ công/tháng**, giảm SLA và NPS ~10–20%.                                                                       |
| 4 | Xanh SM    | Quản lý tài xế & chấm điểm hiệu suất thủ công | Rating + KPI cơ bản, thiếu dữ liệu hành vi (phanh gấp, tốc độ, vòng chạy). Dẫn tới **5–12% giảm hiệu suất vận hành**, tăng rủi ro an toàn và bias trong đánh giá tài xế.                                                                                 |
| 5 | Xanh SM    | Điều phối xe rỗng & repositioning thủ công    | Tài xế tự quyết định di chuyển khi không có khách hoặc theo gợi ý đơn giản → không tối ưu supply redistribution. Gây **12–20% xe di chuyển không tạo doanh thu** trong giờ thấp điểm.                                                                    |


Phase 2

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tối ưu điều phối xe và tài xế theo thời   │
│ gian thực để giảm thời gian chờ và xe chạy rỗng.            │
│                                                             │
│ Công ty thành viên:              [X] Xanh SM                │
│                                                             │
│                                                             │
│ Ai đang đau (Actor)? Dispatcher / hệ thống điều phối        │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│ 1. Nhận yêu cầu khách                                       │
│ 2. Xem vị trí tài xế gần nhất (map)                         │
│ 3. Gán chuyến theo kinh nghiệm / rule đơn giản →            │
│ 4. Điều chỉnh nếu tài xế từ chối →                          │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2–3 (~30–90s/lượt)    │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Ranking tài xế theo   │
│ ETA + traffic + distance                                    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Giảm pickup time: 8 min → 5 min                           │
│ - Giảm idle km: -10–15%                                     │
│                                                             │
│ Quick Architecture: [ ] No AI  [X] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Dự đoán nhu cầu gọi xe theo khu vực để    │
│ tối ưu phân bổ tài xế theo giờ cao điểm.                    │
│                                                             │
│ Công ty thành viên:   [X] Xanh SM                           │
│                                                             │
│                                                             │
│ Ai đang đau (Actor)? Fleet planner / Ops manager            │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│ 1. Xem lịch sử chuyến theo ngày →                           │
│ 2. Ước lượng giờ cao điểm →                                 │
│ 3. Điều xe sang khu hot →                                   │
│ 4. Điều chỉnh theo cảm tính / kinh nghiệm                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (~manual guess)     │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Forecast demand       │
│ theo time-slot + zone                                       │
│                                                             │
│ Metric:                                                     │
│ - giảm mismatch supply-demand 10–20%                        │
│ - tăng utilization fleet                                    │
│                                                             │
│ Quick Architecture: [ ] No AI  [X] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại và xử lý khiếu nại      │
│ khách hàng từ call center / chat.                           │
│                                                             │
│ Công ty thành viên: [ ] VinFast  [X] Xanh SM  [ ] Vinhomes  │
│                                      │
│                                                             │
│ Ai đang đau (Actor)? CSKH / Call center agent               │
│                                                             │
│ Workflow thủ công hiện tại:                                 │
│ 1. Nhận cuộc gọi/chat →                                     │
│ 2. Đọc nội dung →                                           │
│ 3. Tự phân loại loại complaint →                            │
│ 4. Gửi sang team liên quan →                                │
│ 5. Ghi CRM                                                  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (~30–120s/case)     │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Classification +      │
│ routing                                                     │
│                                                             │
│ Metric:                                                     │
│ - giảm handling time 40%                                    │
│ - tăng SLA compliance                                       │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘