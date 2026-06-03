# Evidence Pack - Traveloka Refund/Reschedule AI


## 1. Nhóm và track

**Tên nhóm:** 4Tuner
**Track:** B — Travel & Hospitality
**Product/app đã chọn:** Traveloka (app đặt vé/phòng — đã có AI chatbot + Smart Trip Planner, nhưng luồng refund/reschedule còn yếu)
**Build slice đang nghĩ:** AI hỗ trợ hủy/đổi booking với conditional automation — nói rõ điều kiện/phí/timeline, tự xử lý case rõ ràng, chuyển người khi mơ hồ.

## 2. Self-use evidence


| Observation | Screenshot/link                                                                                                                                                | Path liên quan | Điều học được |
|---|----------------------------------------------------------------------------------------------------------------------------------------------------------------|---|---|
| _(Luồng hủy 1 booking thật → tới bước xác nhận, chụp chỗ chatbot / option lý do hủy)_ | [Trustpilot](https://www.trustpilot.com/review/traveloka.com), [App Store reviews](https://play.google.com/store/apps/details?id=com.traveloka.android&pli=1)  | Failure / Low-confidence |  |
| _(Hỏi chatbot 1 câu refund khó → xem nó chạy vòng hay cho gặp người thật)_ | [Trustpilot](https://www.trustpilot.com/review/traveloka.com), [App Store reviews](https://play.google.com/store/apps/details?id=com.traveloka.android&pli=1)  | Failure |  |
| _(Tìm chỗ hiển thị phí phạt + timeline hoàn tiền → có rõ trước khi bấm không?)_ | [Trustpilot](https://www.trustpilot.com/review/traveloka.com), [App Store reviews](https://play.google.com/store/apps/details?id=com.traveloka.android&pli=1)  | Happy / Correction |  |

## 3. User / review / social evidence

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "chat option sends you in circle using automation, while no real agent sits there to chat with you" | [Trustpilot](https://www.trustpilot.com/review/traveloka.com) | Khách cần hỗ trợ gấp | Chatbot dead-end, không escalate lên người thật |
| Đơn refund (lý do visa bị từ chối) bị reject trong <1 giờ → nghi không có review thật | [App Store reviews](https://apps.apple.com/us/app/traveloka-book-hotel-flight/id898244857?see-all=reviews) · [Tripadvisor](https://www.tripadvisor.com/ShowUserReviews-g294229-d34100870-r1048453234-Traveloka-Jakarta_Java.html) | Khách bị hủy chuyến vì lý do bất khả kháng | AI/quy trình từ chối quá nhanh, không minh bạch lý do |
| App không có option "visa denial" → buộc chọn lý do sai, ảnh hưởng xét duyệt | [justuseapp / App Store](https://justuseapp.com/en/app/898244857/traveloka-flight-hotel-trip/reviews) | Khách có lý do hủy nằm ngoài danh sách | Low-confidence: hệ thống ép chọn sai thay vì hỏi lại |
| Email báo "refund successful" (23/12/2025) nhưng tới 9/1/2026 chưa nhận tiền | [Trustpilot](https://www.trustpilot.com/review/traveloka.com) | Khách đã được duyệt refund | Báo trạng thái sai, không có đường correction/khiếu nại |
| Refund chờ tới 90 ngày; reschedule fee ~110% gần bằng mua vé mới | [PissedConsumer (2.0★)](https://traveloka.pissedconsumer.com/review.html) | Khách đổi/hủy vé | Phí & timeline không được cảnh báo rõ trước khi quyết |
| (VN) Phí xử lý hoàn tiền 10%; hoàn trong 5 ngày nhưng tới 90 ngày tùy ngân hàng | [Traveloka VN — Chính sách hoàn tiền](https://www.traveloka.com/vi-vn/refund/hotel) · [fundi.vn](https://fundi.vn/traveloka-hoan-tien-khong-thoa-dang-cho-khach-hang-la-lua-dao-hay-co-nguyen-nhan-khac/) | Khách VN | Chính sách có thật nhưng user không thấy rõ lúc cần |

## 4. Competitor / analog evidence

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| Trip.com — TripGenie | Trợ lý hội thoại sinh lịch trình ("3-day beach trip Thailand under $400") | Hội thoại tự nhiên + dẫn thẳng tới booking | Một phần (UI hội thoại) |
| Expedia — Romie / Booking.com — AI Trip Planner | AI plan/refine itinerary, so sánh & book ngay trong chat | Gộp tra cứu + hành động vào 1 luồng | Một phần |
| Klook — K.AI + AI shopping guide | Q&A tương tác; AI tóm tắt review để hỗ trợ quyết định | Tóm tắt chính sách/đánh giá để giảm tải quyết định | Có (tóm tắt chính sách phạt) |
| **Khoảng trống chung của OTA** | AI tập trung vào **planning/booking**, hiếm app làm tốt **AI cho refund/reschedule có minh bạch + escalation** | Đây là chỗ chưa ai làm tốt → cơ hội của nhóm | Có — chính là build slice |

## 5. Evidence -> Insight

```text
Evidence nổi bật nhất:
Chatbot Traveloka xử lý ~75% query nhưng review thật cho thấy nó chạy vòng tròn,
từ chối refund quá nhanh, ép chọn lý do hủy sai, và báo "refund successful" giả —
tức AI làm tốt happy path nhưng gãy ở low-confidence / failure / correction.

Insight:
User không chỉ gặp "chatbot trả lời chậm/sai".
Thật ra họ cần một luồng hủy/đổi MINH BẠCH và CÓ LỐI THOÁT:
biết ngay đủ điều kiện không, mất phí bao nhiêu, bao lâu hoàn tiền,
và khi AI không chắc thì được chuyển người thật kèm ngữ cảnh.

Opportunity:
AI có thể augment+automate có điều kiện luồng refund/reschedule:
tra đúng chính sách, giải thích rõ trước khi quyết, tự xử lý case rõ ràng,
escalate case mơ hồ/phí lớn, và không bao giờ báo trạng thái giả.
```

## 6. Evidence đổi SPEC như thế nào?

- [ ] Đổi user chính.
- [x] Đổi pain statement.
- [x] Đổi build slice.
- [x] Đổi Auto/Aug decision.
- [x] Đổi 4 paths.
- [x] Đổi failure mode.
- [ ] Đổi owner/test plan.

```text
Trước evidence, nhóm định: làm "AI gợi ý lịch trình" cho vui (giống Trip.com/Expedia đã có).
Sau evidence, nhóm đổi thành: AI hỗ trợ hủy/đổi booking với minh bạch + escalation —
vì đây là chỗ Traveloka (và đa số OTA) đang gãy thật theo review, ít ai làm tốt.
Lý do: pain có bằng chứng dày, failure path rõ, và Auto/Aug có tranh luận thật (đúng yêu cầu rubric).
```