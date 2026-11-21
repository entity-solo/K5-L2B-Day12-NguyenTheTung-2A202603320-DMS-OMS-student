# Danh sách lớp v0 — Nguyen The Tung · G11

> Đã đối chiếu slide 54, Guideline Cabin v1.0 và thông báo mới của thầy (`child_seat.occupied = yes`). Bản chốt để nộp.

Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face.eye_left` / `eye_right` | open / closed / unknown | Phát hiện buồn ngủ, mắt nhắm lâu; tách "không thấy" khỏi "mí khép" | Có, nhưng đốm phản xạ LED hoặc kính râm → unknown |
| 2 | `face.glasses` | unknown / none / clear / sun | Nhận diện đúng người đeo kính, tránh dạy model rằng đeo kính = không có kính | NIR có thể làm kính râm trông trong suốt → dễ nhầm, không chắc thì unknown |
| 3 | `face.gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Cảnh báo tài xế không nhìn đường, biết hướng nhìn cụ thể | Có (hướng mắt + đầu), other khi không chắc |
| 4 | `face13` (13 điểm + cờ occluded/outside) | 13 điểm mặt | Ước lượng head pose/yaw, hỗ trợ đường nhìn và cảnh báo mất chú ý | Có khi mặt ≥ 60 px; điểm bị che phải bật cờ, không đoán |
| 5 | `hand.whose` / `side` / `on_wheel` | driver/passenger · left/right · yes/no/unknown | Phát hiện tay rời vô lăng, tay cầm vật, thao tác của tài xế | Có; trái/phải theo người, ảnh xoay 90° vẫn giữ quy ước |
| 6 | `phone.state` | held / mounted / lap | Phát hiện dùng điện thoại khi lái | Có khi thấy cạnh/góc/màn hình; không thấy thì không vẽ |
| 7 | `person.seatbelt` | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây an toàn đúng cách | Có thể tối hoặc bị che → unknown, không suy từ khung khác |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person.seat` | driver / front_passenger / rear_left / rear_middle / rear_right | Biết ai ngồi ghế nào, phục vụ nhắc nhở và cấu hình túi khí | Có (hình dạng); IR-sim giữ được bố cục ghế |
| 2 | `person.seatbelt` | not_worn / unknown (SVIRO không có `worn_*`) | Nhắc thắt dây; đồng thời tránh dạy model lớp không có trong dữ liệu | IR-sim đôi khi tối hơn RGB → để unknown nếu không thấy |
| 3 | `face` (chỉ box, không thuộc tính) | có / không | Xác nhận có người ở ghế đó dù thân thể bị che | Có (hình dạng), box sát mép |
| 4 | `child_seat.occupied` | no / unknown / yes (theo thông báo mới của thầy) | Nhắc ghế trẻ em; phân biệt ghế trống (`no`), không rõ (`unknown`) và có vật/trẻ trong ghế (`yes`) | Có (hình dạng); theo guideline, có bất kỳ cái gì nằm trong ghế thì `occupied = yes` |
| 5 | `infant_seat.facing` | rear / front | Nhắc quên em bé, quyết định hướng/tắt túi khí | Có (hình dạng nôi) |
| 6 | `object.type` | bag / box / cup / other | Nhận đồ mang theo, cảnh báo vật cản hoặc quên đồ trên xe | Có (hình dạng + vị trí); không chắc → other |
| 7 | `frame_meta.camera_blocked` | yes / no | Lọc khung bị che trước khi huấn luyện, tránh nhiễu | IR-sim là ảnh dựng nên không tái hiện đúng một số hiệu ứng IR thật |

## 2 câu hỏi leo thang

1. Câu hỏi: Khi vật nằm trong ghế trẻ em che gần hết bên trong, guideline bắt vẽ 2 box (`child_seat` + `object`); vậy `occupied` nên là `no` hay `unknown`, và có được phép bỏ box `child_seat` khi không thấy rõ khung ghế không?
   Khung ví dụ: `G11 S04_ir` (khung minh họa theo mẫu; bộ G11 không phát sinh ca này).
   Bạn tạm gán thế nào và vì sao: trong G11 không có ca này. Nếu có, theo guideline thì **bất kỳ cái gì nằm trong ghế đều là `occupied = yes`** (giá trị đã được thầy xác nhận hợp lệ). Vẫn vẽ đủ 2 box `child_seat` + `object` khi thấy hình dạng ghế; không bỏ box `child_seat`. Nếu không thấy rõ khung ghế thì ghi chú và không vẽ `child_seat`.

2. Câu hỏi: `cabin_check` cảnh báo trái/phải bằng trục x trong khi ảnh `D##` xoay ~90°; guideline nên sửa script để so theo trục y (mắt trái y lớn hơn) hay giữ cảnh báo và bắt người gán tự giải thích?
   Khung ví dụ: `G11 D04` (khung minh họa; sau khi sửa D04–D06, G11 hiện không còn WARN trái/phải).
   Bạn tạm gán thế nào và vì sao: giữ quy ước trái/phải theo người (luật 4.4-2). Trên ảnh `D##` xoay ~90°, **mắt ở phía thấp hơn của ảnh (y lớn hơn) là mắt trái** — mốc này dùng khi mắt còn nhìn thấy. Trục `x` nhỏ chỉ dùng để đặt điểm mí dưới của `face13` (luật 4.6), không dùng để chọn mắt trái. Script được thầy cho phép tự sửa theo rule mới (đã thêm `yes` vào `child_seat.occupied` trong `cabin_schema.py`); nếu còn WARN thì ghi 1 dòng giải thích vào `check_final.txt` như `CHECKPOINTS.md` hướng dẫn.
