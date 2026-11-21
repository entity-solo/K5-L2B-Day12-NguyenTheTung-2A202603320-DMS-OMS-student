# Tự đánh giá — Nguyen The Tung · G11

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| D03.png | `face13` (điểm mắt) | `flag` | Mắt khó thấy: chỗ còn dấu hiệu thì đánh `occluded`, chỗ không đoán được thì đánh `outside`; không đặt điểm "cho đủ" (luật 4.6) | có |
| D04.png | `phone` | `guess` | Giữ box vì thao tác áp tai và cách dùng khá giống điện thoại; tuy luật 4.7 còn cần thấy cạnh/góc/màn hình nên vẫn ghi nhận là ca mơ hồ | có |
| S01_rgb.png | `object` | `ok` | Vật không rõ là gì nên chọn `type=other`, không đoán `bag`/`cup` (luật 4.4-1) | có |
| S08_ir.png | `infant_seat` | `ok` | Hai loại ghế trẻ em dễ nhầm; thấy quai nôi nên chọn `infant_seat`, không chọn `child_seat` (luật 4.2) | có |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: `guess` / `flag` ở vùng mắt trên các ảnh D##.

Vì sao mình mắc: ảnh NIR xoay khoảng 90°, mắt thường bị che hoặc có đốm phản xạ nên rất dễ đoán `open` hoặc đặt điểm `face13` "cho đủ".

Lần sau tránh bằng cách: áp luật 4.4-1 (không thấy rõ thì `unknown`) và luật 4.6 (còn dấu hiệu mắt → `occluded`; không đoán được vị trí → `outside`).

## 3. Ghi chú về disagreement_log

Ca mơ hồ cần hỏi coach: `D04.png` – `face.gaze_zone` (mình `center`, peer `mirror_l`). Mình đã ghi nhận ý peer để đối chiếu lại. Các dòng còn lại trong `disagreement_log.csv` đã chốt `verdict=mine` theo bài mình làm chuẩn.
