# Thin SPEC - Traveloka Refund/Reschedule AI


## 1. Track, product/app và user

**Track:** B — Travel & Hospitality
**Product/app thật:** Traveloka (đã có AI chatbot + Smart Trip Planner; luồng refund/reschedule còn yếu)
**User cụ thể:** Khách đã đặt phòng/vé trên Traveloka, cần **hủy hoặc đổi** vì lý do bất khả kháng (visa bị từ chối, ốm, đổi lịch công tác) và đang bối rối về điều kiện/phí.
**Nhóm có phải user thật không? Nếu không, khác ở đâu?** Nhóm là user thật ở mức "đã từng đặt/hủy", nhưng chưa trải đủ case bị từ chối refund → phải dựa thêm review thật để không tự bịa pain.

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| "chat sends you in circle, no real agent" | [Trustpilot](https://www.trustpilot.com/review/traveloka.com) | Chatbot không có lối thoát lên người thật | Bắt buộc có path escalate → người |
| Refund bị reject <1h, không rõ lý do | [App Store](https://apps.apple.com/us/app/traveloka-book-hotel-flight/id898244857?see-all=reviews) · [Tripadvisor](https://www.tripadvisor.com/ShowUserReviews-g294229-d34100870-r1048453234-Traveloka-Jakarta_Java.html) | Quyết định mờ ám, mất niềm tin | AI phải giải thích lý do + dẫn nguồn chính sách |
| App thiếu option lý do hủy (visa denial) | [justuseapp](https://justuseapp.com/en/app/898244857/traveloka-flight-hotel-trip/reviews) | Bị ép chọn lý do sai | Low-confidence: hỏi lại thay vì ép chọn |
| Email "refund successful" nhưng không nhận tiền | [Trustpilot](https://www.trustpilot.com/review/traveloka.com) | Báo trạng thái giả | Cấm báo "thành công" khi chưa chắc; có đường khiếu nại |
| Phí 10% + reschedule ~110% + tới 90 ngày | [PissedConsumer](https://traveloka.pissedconsumer.com/review.html) · [Traveloka VN](https://www.traveloka.com/vi-vn/refund/hotel) | Phí/timeline bất ngờ | Hiện rõ phí + timeline TRƯỚC khi user xác nhận |

## 3. Pain statement

```text
Khách đã đặt phòng/vé Traveloka muốn hủy/đổi vì lý do bất khả kháng
đang gặp khó ở bước liên hệ hỗ trợ và xin hoàn tiền,
vì chatbot chạy vòng tròn không cho gặp người, không nói rõ điều kiện/phí/timeline,
ép chọn lý do hủy sai, và đôi khi báo trạng thái giả,
dẫn tới mất tiền, mất thời gian và mất niềm tin.
Bằng chứng chính là review thật trên Trustpilot, App Store, Tripadvisor, PissedConsumer.
```

## 4. Build slice

```text
Cho khách đã đặt phòng khách sạn Traveloka đang muốn hủy/đổi vì lý do bất khả kháng,
prototype sẽ dùng AI để: nhận mã booking + lý do → tra đúng chính sách hủy →
giải thích rõ NGAY (đủ điều kiện không / phí bao nhiêu / bao lâu hoàn tiền),
tạo ra một "thẻ quyết định" minh bạch + hành động conditional automation
(tự xử lý nếu rõ ràng & miễn phí; chuyển người kèm ngữ cảnh nếu mơ hồ/phí lớn/mã sai),
và xử lý failure mode "AI bịa chính sách hoặc mã sai" bằng dẫn nguồn + escalate, không auto-từ-chối.
```

## 5. Auto/Aug decision

Chọn một:

- [ ] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [x] **Conditional automation:** AI tự làm trong case hẹp; case mơ hồ/rủi ro chuyển người.
- [ ] **Automation:** AI tự quyết và tự hành động.

**Lý do chọn:** Refund liên quan tiền + chính sách phạt → rủi ro cao, không thể full-auto; nhưng case rõ ràng (đủ điều kiện, miễn phí hủy) thì auto giúp nhanh và giảm tải chatbot. Conditional automation cân bằng tốc độ và an toàn.
**Human role:** **rescuer + decider** - nhân viên thật nhận case mơ hồ/phí lớn kèm toàn bộ ngữ cảnh AI đã thu thập (không bắt khách kể lại từ đầu).

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| **Happy** | Mã đúng + chính sách rõ + miễn phí hủy → AI xác nhận điều kiện, hiện phí=0 + timeline, cho phép xác nhận hủy/đổi trong 1 bước. |
| **Low-confidence** | Lý do hủy nằm ngoài danh sách (vd visa denial) hoặc chính sách không rõ → AI **hỏi lại** để làm rõ, không ép chọn lý do sai; gắn nhãn "cần xác minh". |
| **Failure** | Mã booking sai / AI không tìm thấy chính sách → báo rõ "chưa xác định được", **dẫn nguồn** + chuyển người, **không** auto-từ-chối, **không** báo trạng thái giả. |
| **Correction** | User phản hồi "thông tin sai" hoặc muốn khiếu nại → có nút khiếu nại/escalate; mọi quyết định AI được log để người review và để học lại. |

## 7. Failure mode nguy hiểm nhất

```text
Nếu user nhập mã booking sai hoặc hỏi về chính sách app không có dữ liệu,
AI có thể bịa ra điều kiện/phí/timeline hoặc tự "từ chối" yêu cầu,
hậu quả là khách mất tiền oan hoặc bỏ lỡ hạn hủy miễn phí, mất niềm tin và khiếu nại.
Prototype sẽ xử lý bằng: show source (dẫn đúng điều khoản chính sách) +
human review (escalate kèm ngữ cảnh) + fallback "chưa xác định được, đang chuyển hỗ trợ",
tuyệt đối không auto-từ-chối và không báo "thành công" khi chưa chắc.
Owner kiểm thử path này là [Hưng, Việt Anh, Toàn, Lân].
```

## 8. Owner plan cho sáng Day 06

| Thành viên                | Việc phụ trách | Bằng chứng cần có trong repo |
|---------------------------|---|---|
| Hưng, Việt Anh, Toàn, Lân | Research / evidence | evidence-pack.md + screenshot self-use |
| Hưng, Việt Anh, Toàn, Lân | SPEC | thin-spec.md hoàn chỉnh |
| Hưng, Việt Anh, Toàn, Lân | Prototype | luồng hủy/đổi chạy được (mã → thẻ quyết định) |
| Hưng, Việt Anh, Toàn, Lân | Test / failure path | log test mã sai + AI bịa chính sách |
| Hưng, Việt Anh, Toàn, Lân | Demo script / repo | kịch bản demo 3-5 phút + README repo |