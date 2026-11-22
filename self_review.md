# Tự đánh giá — Vũ Tuyết Trinh (2A202603164) · G10

## 1. Tự rà soát 4 cảnh

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| D01 | `object` (bag, front_passenger) | `extra` | DMS không vẽ `object` (4.2). Cùng lỗi ở D02–D06, tổng 7 box | Có (v2) |
| D03 | `face13` | `missing` | Mặt rộng ~140 px (> 60 px) nhưng chưa vẽ `face13` (4.6). Cùng lỗi ở D01 | Có (v2) |
| S02_rgb / S02_ir | `child_seat` ghế giữa | `wrong_class` | Ghi `occupied = yes`, guideline chỉ cho `no`/`unknown` (4.8). Ghế giữa có hình giống trẻ em, cần hỏi coach (GUIDE §3) | Có (v2): `occupied = unknown`; box `person` giữ lại, chờ coach |
| S03_ir | `person` ghế giữa, `seatbelt` | `guess` | IR khó nhìn hơn RGB nhưng lại ghi `not_worn`, trong khi RGB ghi `unknown`. Đúng phải là `unknown` (4.4-1) | Không |
| S01–S08 (cả RGB và IR) | mọi box OMS, thuộc tính `seat` | `seat_flip` | Phát hiện khi review chéo với Hà Tiến Trịnh: vật bên trái ảnh mình ghi `rear_left`, đúng theo xe phải là `rear_right` (4.4-3). Lật ở tất cả ảnh S##, `cabin_check` không bắt được | Có (v2) |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Vẽ box `object` trên ảnh tài xế D## (7 box trên cả 6 ảnh D01–D06).

Vì sao mình mắc: Làm OMS trước nên quen tay vẽ `object` cho mọi đồ vật thấy được, khi sang DMS không đổi lại danh sách label được phép. `cabin_check` cũng không báo lỗi này nên mình không phát hiện sớm.

Lần sau tránh bằng cách: Trước khi gán mỗi phần, đọc lại bảng 4.1 cột "Dùng ở" và mục 4.2 "Không gán". Với ảnh D## chỉ dùng `frame_meta`, `person`, `face`, `face13`, `hand`, `phone`. Sau khi gán xong, lọc danh sách đối tượng theo label để soát.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: S01_rgb `person` — mình ghi `rear_right`, Trịnh ghi `rear_left`, Trịnh đúng (4.4-3: camera OMS nhìn về phía sau, trái ảnh = phải xe). Mình đã lật ghế ở toàn bộ S01–S08 mà không nhận ra, vì `cabin_check` không có đáp án ghế. Lần sau: trước mỗi ảnh S## nói to ghế theo xe như 4.3 bước 2, và soát lại bằng quy tắc "box có tâm ở nửa trái ảnh thì phải là `rear_right`".

Tổng kết review chéo: 105 dòng (29 dòng Trịnh đúng, 4 dòng mình đúng, 2 dòng cả hai sai, 70 dòng mơ hồ cần xem lại trên CVAT hoặc hỏi coach). Sau review chéo mình đã sửa trên CVAT và nộp `export_final_v2.zip`: đổi ghế OMS theo xe ở S01–S08, xoá box `object` trên D01–D06, thêm `face13` cho D01 và D03, sửa S02 `occupied = unknown`. `check_final_v2.txt` báo 0 lỗi, 3 cảnh báo đã giải thích. Chưa sửa: S03_ir `seatbelt` (vẫn `not_worn`) và các box `person` trong ghế trẻ em/em bé ở S02, S03, S05, S07 (chờ coach).
