# Synthesis → Build Slice — Traveloka Refund/Reschedule AI


## 1. Gom evidence thành cụm

Gom theo workflow/pain (không gom theo feature):

- **"Chatbot chạy vòng tròn, không gặp được người"** — review Trustpilot/PissedConsumer.
- **"Không biết mình đủ điều kiện refund không / mất phí bao nhiêu / bao lâu"** — phí 110% reschedule, 10% xử lý, tới 90 ngày.
- **"Bị ép chọn lý do hủy sai"** — app thiếu option (visa denial).
- **"Bị báo trạng thái giả, không có chỗ khiếu nại"** — email "refund successful" nhưng không nhận tiền.

→ Cả 4 cụm đều rơi vào **low-confidence / failure / correction** — không phải happy path.

## 2. Insight

```text
User Traveloka đang hủy/đổi booking không chỉ cần một chatbot trả lời nhanh.
Họ thật ra cần một luồng minh bạch và có lối thoát:
biết ngay điều kiện + phí + timeline, và được chuyển người thật khi AI không chắc,
vì review cho thấy chatbot hiện chạy vòng tròn, từ chối vô lý và báo trạng thái giả.
```

## 3. Opportunity

```text
Cơ hội là dùng AI để augment + automate-có-điều-kiện luồng hủy/đổi booking:
tra đúng chính sách và giải thích rõ trước khi user quyết,
tự xử lý case rõ ràng (đủ điều kiện, miễn phí), chuyển người case mơ hồ/phí lớn,
giúp user lấy lại tiền/đổi lịch mà không bị kẹt,
trong khi vẫn kiểm soát rủi ro AI bịa chính sách bằng việc bắt buộc dẫn nguồn.
```

## 4. Chọn build slice — kiểm 5 câu hỏi

**Build slice:** AI hỗ trợ hủy/đổi 1 booking Traveloka (mã booking + lý do → giải thích điều kiện/phí/timeline → conditional automation).

| Câu hỏi | Đạt khi | Trạng thái |
|---|---|---|
| User cụ thể chưa? | Khách đã đặt phòng/vé Traveloka, muốn hủy/đổi vì lý do bất khả kháng (visa, ốm, đổi lịch). | ✅ |
| Task đủ hẹp chưa? | Chỉ luồng hủy/đổi 1 booking, demo 3-5 phút. | ✅ |
| AI decision rõ chưa? | AI tra chính sách → quyết: tự xử lý / cần người / từ chối kèm lý do. | ✅ |
| Failure path rõ chưa? | Mã sai HOẶC AI bịa chính sách → dẫn nguồn + escalate, không auto-từ-chối. | ✅ |
| Có evidence không? | Review thật Trustpilot/PissedConsumer/App Store/Tripadvisor (xem evidence-pack). | ✅ |

## 5. Quyết định: giữ, giảm scope, hay đổi hướng?

| Tình huống | Áp dụng cho nhóm? | Quyết định |
|---|---|---|
| Evidence yếu, user mơ hồ | Không — evidence dày | Giữ |
| Ý tưởng quá rộng | Một phần (Traveloka làm cả vé+phòng+activity) | **Cắt xuống đúng 1 flow: hủy/đổi** |
| AI không cần thiết | Không — cần NLP hiểu lý do + RAG chính sách | Giữ AI |
| Rủi ro cao (tiền/refund) | **Có** | **Conditional automation** — auto case rõ, người giữ quyền case mơ hồ/phí lớn |
| Không demo được trong 1 ngày | Phần xử lý thanh toán thật → backlog | Giữ phần giải thích + quyết định + escalation |

## 6. Câu chốt cuối

```text
Dựa trên review thật về chatbot chạy vòng tròn + refund mờ ám của Traveloka,
nhóm sẽ build prototype "AI hỗ trợ hủy/đổi booking",
cho khách đã đặt phòng/vé Traveloka muốn hủy/đổi vì lý do bất khả kháng,
để giải quyết pain "không biết điều kiện/phí/timeline và bị chatbot bỏ rơi",
bằng cách AI tra chính sách + giải thích rõ + tự xử lý case rõ ràng / chuyển người case mơ hồ (conditional automation),
và sẽ test failure path: mã booking sai HOẶC AI bịa chính sách → dẫn nguồn + escalate, không auto-từ-chối.
```

## 7. Backlog (KHÔNG build trong Day 06)

- Tích hợp thanh toán/hoàn tiền thật (chỉ mô phỏng tới bước quyết định + xác nhận).
- Đa dịch vụ (vé máy bay, activity) — Day 06 chỉ làm 1 loại (vd: phòng khách sạn).
- Đa ngôn ngữ / voice — chỉ làm text trước.
- Tài khoản thật + đăng nhập — dùng booking mẫu.