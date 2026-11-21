# Khai báo dùng AI — Nguyen The Tung

**Không gửi ảnh cabin / ảnh tài xế / ảnh chụp màn hình lên dịch vụ AI nào.** Phần pre-annotate chạy local trên máy, chỉ tạo gợi ý và do bạn tự rà soát.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | OpenCode (AI agent) | Đọc tài liệu repo Day12 để lập lộ trình: clone repo, giải nén G11, bật CVAT, cú pháp `cabin_check` ở 3 checkpoint, cách điền template | Chỉ dùng để chuẩn bị môi trường, giải thích guideline và cú pháp lệnh | Có — đối chiếu trực tiếp với `GUIDE.md`, `CHECKPOINTS.md`, `RULES.md`, `SUBMISSION.md` trong repo |
| 2 | OpenCode (AI agent) + model chạy local (YOLO11s, MediaPipe Face/Hand Landmarker, YOLO worldv2) | Nhờ pre-annotate 22 ảnh và xử lý lỗi CVAT/API | Chỉ tạo shape gợi ý `source=auto`, sửa schema label trên CVAT (thêm `yes` cho `child_seat.occupied` theo file `Downloads` và thông báo của thầy). Bạn tự soát/sửa từng box trước khi export; ảnh không gửi lên dịch vụ AI | Có — chạy `cabin_check`, tự sửa 3 lỗi tay trên D04/D05/D06, đối chiếu rule mới của thầy về `occupied=yes` |
