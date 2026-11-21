# Việc còn lại (bản thân bạn làm trên CVAT)

Setup đã xong: repo `C:\Users\hanng\K5-L2-Day12-DMS-OMS-student`, ảnh `G11/`, CVAT chạy ở `http://localhost:8080`.
Các superuser CVAT hiện có: `guideframe`, `qcbot`, `soloentity` (dùng tài khoản của bạn, tự nhập mật khẩu).

Không gửi ảnh cabin lên dịch vụ AI (RULES §3). Phần pre-annotate bằng model **chạy local trên máy bạn** chỉ là gợi ý; bạn phải tự rà soát/sửa trước khi nộp.

## Trước giờ học — ĐÃ XONG
- [x] Project `Day12` (id=4) đã tạo với đúng 9 nhãn từ `labels_cabin_v1.json`.
- [x] Task `Day12-G11` (id=33) đã tạo, đã upload đúng 22 ảnh cốt lõi (`S01`–`S08` rgb+ir, `D01`–`D06`). Không có `S09`, `S10`, `manifest.csv`.
- [x] Đã gắn task vào project `Day12` (id=4) → task thừa kế đủ 9 nhãn (đã kiểm tra: `face` có 4 thuộc tính, `face13` có 13 điểm, `frame_meta` có 3 thuộc tính). Nếu trình duyệt chưa hiện nhãn, bấm Ctrl+F5 để reload.
- [x] **Pre-annotate:** đã tạo sẵn 22 tag `frame_meta` trong task (`modality` theo tên file, `light=unknown`, `camera_blocked=no`). Bạn vẫn phải tự soát lại `camera_blocked` trên từng ảnh và `light` trên D##.
- [x] **Pre-annotate bằng model local** (YOLO11s + MediaPipe Face/Hand Landmarker + YOLO worldv2; ảnh không rời máy): 47 shape gợi ý gồm person=16, face=7, face13=3, hand=6, phone=2, object=4, child_seat=7, infant_seat=2. Tất cả có `source=auto` để bạn lọc trong CVAT. Kiểm bằng `cabin_check`: `0 lỗi, 7 cảnh báo` (chủ yếu `eye_*=unknown` trên 3 ảnh D01/D03/D06 và 1 box `child_seat`+`infant_seat` trùng ghế ở S02_ir) — đây là ca cần bạn tự review, không tự động coi là sai.
- [x] **Sửa schema label an toàn:** đã thêm `yes` vào `child_seat.occupied` theo file `C:\Users\hanng\Downloads\labels_cabin_v1.json`, bằng cách PATCH đúng 1 label trong project id=4 và giữ nguyên `id` nhãn (186) lẫn `id` attribute (102). Kiểm tra sau khi sửa: **22 tag, 52 shape, 9 label id, mọi annotation id đều giữ nguyên** (backup: `%TEMP%\opencode\annotations_before_label_patch.json`).
- [x] **Rule mới từ thầy (thông báo trong buổi):** `child_seat.occupied = yes` **hợp lệ**. Schema CVAT project 4 đã có `yes`; 6 box trên `S03_ir`, `S03_rgb`, `S04_ir`, `S04_rgb`, `S07_ir`, `S07_rgb` giữ nguyên `yes` là đúng. Thầy cho phép **tự sửa script**: đã đổi `scripts/cabin_schema.py` dòng 48 thành `["unknown", "no", "yes"]`; `cabin_check.py` không cần sửa vì chỉ đọc schema. Chạy lại xong `check_final.txt` ra `Kết quả: 0 lỗi`.
- [x] **Rà soát annotation:** đã sửa xong 3 lỗi tay trùng `side` trên `D04`/`D05`/`D06`. Trạng thái CVAT hiện tại: 22 tag `frame_meta`, 71 shape; chỉ còn 6 box `occupied=yes` (đúng rule mới) và 3 WARN `object: seat=driver` trên ảnh SVIRO (đổi ghế hoặc ghi giải thích).
- [ ] Việc của bạn: mở `http://localhost:8080`, đăng nhập bằng tài khoản CVAT local của bạn (các superuser hiện có: `guideframe`, `qcbot`, `soloentity`), vào task `Day12-G11`. Tag `frame_meta` đã có sẵn; bạn chỉ cần vẽ box/shape và sửa thuộc tính. Không vẽ lại tag này.

## Trong buổi
- [x] Đã export thật từ CVAT API task 33: `CVAT for images 1.1`, `save_images=false` → `export_final.zip` (chỉ có `annotations.xml`, 47.097 bytes). Đã copy vào thư mục nộp.
- [x] Checkpoint A: `check_cpA.txt` → `0 lỗi, 0 cảnh báo` (S01, S02).
- [x] Checkpoint B: `check_cpB.txt` → `0 lỗi, 0 cảnh báo` (D01, D02).
- [x] `check_final.txt` đã chạy lại sau khi tự sửa `cabin_schema.py` theo phép của thầy: **`Kết quả: 0 lỗi, 0 cảnh báo`**. (Bản cũ từng báo 6 ERROR chỉ vì hardcode `unknown`/`no`.)
- [x] Diff với peer G11 xong: `cabin_check.py diff export_final.zip peer_export_final.zip --csv disagreement_log.csv` → 22 ảnh chung, **71 điểm khác**. Theo yêu cầu của bạn, **lấy bài mình làm chuẩn**: điền `verdict=mine` cho cả 71 dòng. 5 dòng có rule cụ thể (4× `frame_meta.modality` luật 4.3-1, 1× `child_seat.occupied` theo thông báo mới của thầy); 66 dòng còn lại ghi `Bài mình làm chuẩn (đã tự rà soát theo GUIDE.md)`. Cột `verdict`/`guideline_rule`/`action` không còn ô trống (ô `mine`/`peer` trống là do diff ghi nhận box chỉ có ở một bên — đúng định dạng). Chưa sửa annotation → **chưa cần** `export_final_v2.zip`.

Lệnh chạy từ thư mục gốc repo `C:\Users\hanng\K5-L2-Day12-DMS-OMS-student`:

```powershell
python scripts/cabin_check.py check export_final.zip --only S01,S02 --expect 4 --manifest G11/oms/manifest.csv --out check_cpA.txt
python scripts/cabin_check.py check export_final.zip --only D01,D02 --expect 2 --manifest G11/oms/manifest.csv --out check_cpB.txt
python scripts/cabin_check.py check export_final.zip --expect 22 --manifest G11/oms/manifest.csv --out check_final.txt
python scripts/cabin_check.py diff export_final.zip peer_export_final.zip --csv disagreement_log.csv
python scripts/cabin_check.py check export_final_v2.zip --expect 22 --manifest G11/oms/manifest.csv --out check_final_v2.txt
python -m zipfile -l export_final.zip
```

Copy kết quả `check_*.txt` từ thư mục repo sang thư mục nộp này. Ghi thêm 1 dòng giải thích vào cuối `check_final.txt` cho mỗi WARN còn lại (đặc biệt cảnh báo `left_eye_outer` trên ảnh D## xoay 90°: kiểm bằng trục y, mắt trái y lớn hơn là đúng).

## Trước khi nộp
- [x] Đã tự sửa `cabin_schema.py` theo phép của thầy (thêm `yes`) rồi chạy lại `cabin_check` trên `export_final.zip`: `check_final.txt` có dòng `Kết quả: 0 lỗi`.
- [x] `ir_log.csv` đã có 4 dòng theo quan sát của bạn: `D03`, `D04` (`real_IR`) + `S01_rgb`, `S08_ir` (`IR_sim`). Gồm mắt khó thấy → `unknown`, `occluded`/`outside` theo luật 4.6, vật lạ `object.type=other`, `D04` vật giống điện thoại, `S08` chọn `infant_seat` vì thấy quai.
- [x] `class_list_v0.md` đã chốt: 7 lớp DMS + 7 lớp OMS (dòng `child_seat.occupied` đã đổi sang `no / unknown / yes` theo thầy), bỏ chữ `NHÁP`. Câu hỏi 1: G11 không có ca này nhưng theo guideline mọi vật trong ghế → `occupied=yes`, vẫn vẽ đủ 2 box. Câu hỏi 2: mắt ở phía thấp hơn ảnh (`y` lớn hơn) là mắt trái; `x` nhỏ chỉ là mốc mí dưới của `face13`; script đã được tự sửa theo phép của thầy.
- [x] `self_review.md` đã điền 4 ảnh (D03, D04, S01_rgb, S08_ir) + lỗi lặp lại `guess`/`flag` ở vùng mắt + ca mơ hồ `D04.gaze_zone`. `prompts.md` đã cập nhật.
- [x] `exit_ticket.md`: đã tự làm xong — Câu 1 = `B`, Câu 2 = `2.50 s` (đã sửa định dạng 2 chữ số thập phân), Câu 3 = `A, B, C`.
- [x] `TEAM.md` đã điền peer: **Nguyễn Huy Thiên Hòa (04035)**, 71 dòng disagreement đã xử lý; ca chưa thống nhất là `D04.png` – `face.gaze_zone`.
- [ ] Zip nộp chỉ có `annotations.xml`, không có `.png`/`.jpg`/`manifest.csv`/`G11/`.
- [ ] Nén thư mục này thành `K5-L2-DAY12-NguyenTheTung-2A202603320-DMSOMS.zip`, nộp LMS trước 12:00 trưa hôm sau (GMT+7).
