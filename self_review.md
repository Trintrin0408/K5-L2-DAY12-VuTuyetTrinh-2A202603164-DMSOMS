# Tự đánh giá — Vũ Tuyết Trinh (2A202603164) · G10

## 1. Tự rà soát 4 cảnh

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| D01 | `object` (bag, front_passenger) | `extra` | DMS không vẽ `object` (4.2). Cùng lỗi ở D02–D06, tổng 7 box | Không |
| D03 | `face13` | `missing` | Mặt rộng ~140 px (> 60 px) nhưng chưa vẽ `face13` (4.6). Cùng lỗi ở D01 | Không |
| S02_rgb / S02_ir | `child_seat` ghế giữa | `wrong_class` | Ghi `occupied = yes`, guideline chỉ cho `no`/`unknown` (4.8). Ghế giữa có hình giống trẻ em, cần hỏi coach (GUIDE §3) | Không |
| S03_ir | `person` ghế giữa, `seatbelt` | `guess` | IR khó nhìn hơn RGB nhưng lại ghi `not_worn`, trong khi RGB ghi `unknown`. Đúng phải là `unknown` (4.4-1) | Không |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Vẽ box `object` trên ảnh tài xế D## (7 box trên cả 6 ảnh D01–D06).

Vì sao mình mắc: Làm OMS trước nên quen tay vẽ `object` cho mọi đồ vật thấy được, khi sang DMS không đổi lại danh sách label được phép. `cabin_check` cũng không báo lỗi này nên mình không phát hiện sớm.

Lần sau tránh bằng cách: Trước khi gán mỗi phần, đọc lại bảng 4.1 cột "Dùng ở" và mục 4.2 "Không gán". Với ảnh D## chỉ dùng `frame_meta`, `person`, `face`, `face13`, `hand`, `phone`. Sau khi gán xong, lọc danh sách đối tượng theo label để soát.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: … (điền sau review chéo)
