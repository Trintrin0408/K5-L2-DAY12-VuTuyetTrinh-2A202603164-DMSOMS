# Danh sách lớp v0 — Vũ Tuyết Trinh (2A202603164) · G10

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | eye_state (từng mắt) | open / closed / unknown | Phát hiện buồn ngủ: mắt nhắm kéo dài qua nhiều khung thì cảnh báo | Có, trừ khi kính phản xạ, mặt nghiêng che một mắt, hoặc ảnh tối (D01, D03, D04, D05) |
| 2 | gaze_zone | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện mất tập trung: tài xế không nhìn đường quá lâu (nhìn đùi, màn hình giữa) | Có, dựa vào hướng đầu và mắt; kém tin cậy khi không thấy mắt |
| 3 | phone | held / mounted / lap | Phát hiện dùng điện thoại khi lái (cầm tay, áp tai) để cảnh báo | Có khi thấy cạnh, góc hoặc màn hình máy; vật tối áp tai mà không thấy hình dạng thì không kết luận được |
| 4 | hand.on_wheel | yes / no / unknown (kèm side, whose) | Kiểm tra tài xế sẵn sàng nhận lại quyền lái khi xe chuyển từ tự lái về lái tay | Có (hình dạng tay); unknown khi tay bị che |
| 5 | glasses | none / clear / sun / unknown | Biết khi nào eye_state kém tin cậy (kính phản xạ, kính râm), để hệ thống chuyển sang dùng hướng đầu | Một phần: trên NIR kính râm có thể trông trong suốt, khó tách clear với sun |
| 6 | seatbelt (tài xế) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc tài xế thắt dây, phát hiện thắt sai (dưới nách, sau lưng) | Phần lớn có; unknown khi tối hoặc bị tay, áo che |
| 7 | face13 landmarks | 13 điểm, cờ occluded / outside | Tính độ mở mắt, khóe miệng (ngáp) và hướng đầu, làm đầu vào cho eye_state và gaze_zone | Có khi mặt ≥ 60 px; điểm bị che phải có cờ, mặt nghiêng làm khuất một bên mắt |

## OMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | person + seat | driver / front_passenger / rear_left / rear_middle / rear_right | Đếm người theo từng ghế, làm cơ sở cho nhắc dây an toàn và điều chỉnh túi khí theo ghế | Có (hình dạng cơ thể); ghế gọi theo xe, không theo ảnh |
| 2 | infant_seat + facing | rear / front, theo ghế | Tắt túi khí ghế trước khi nôi quay ngược chiều; nhắc quên trẻ trong xe | Có (hình dạng nôi) |
| 3 | object + type | bag / box / cup / other, theo ghế | Nhắc quên đồ khi xuống xe; phân biệt đồ vật với người để không báo nhầm có người | Có nhưng mất màu, chỉ nhận bằng hình dạng và vị trí (S03_ir); không chắc thì other |
| 4 | face (OMS) | box | Xác nhận ghế có người thật, ước lượng tư thế ngồi, làm đầu vào cho các lớp mặt sau này | Có khi mặt hướng về camera; không vẽ khi không thấy mắt lẫn mũi |
| 5 | empty_seat (lớp mới) | theo ghế | Biết ghế nào còn trống để đếm chỗ, và để model học rõ "không có ai" thay vì chỉ thiếu box | Có |
| 6 | child (lớp mới) | theo ghế, kèm child_seat | Phát hiện trẻ ngồi trong ghế trẻ em: tắt túi khí, cảnh báo quên trẻ trong xe. Guideline v1.0 chưa có lớp này | Có (hình dạng), nhưng khó tách trẻ với người lớn nhỏ người |

## Câu hỏi leo thang

Câu hỏi muốn hỏi người viết guideline vì guideline v1.0 chưa trả lời.

1. Câu hỏi: Khi thấy hình giống trẻ em ngồi trong ghế trẻ em thì gán thế nào? `child_seat` không có `occupied = yes` và guideline cũng không có lớp `child`.
   Khung ví dụ: G10 S02_rgb / S02_ir, ghế `rear_middle`.
   Bạn tạm gán thế nào và vì sao: Mình đã gán `child_seat` với `occupied = yes` và một box `person`, vì muốn ghi lại là ghế có người. Giá trị này sai guideline (`cabin_check` báo lỗi). Theo GUIDE §3 lẽ ra phải báo coach và không gán; nếu được thêm lớp, đề xuất lớp `child` ở bảng OMS #6.
2. Câu hỏi: Mặt rộng trên 60 px nhưng ảnh tối, không thấy rõ cả hai mắt sau kính trong, thì vẫn vẽ đủ `face13` với cờ `outside` cho các điểm mắt, hay được bỏ `face13`?
   Khung ví dụ: G10 D01, D03.
   Bạn tạm gán thế nào và vì sao: Mình ghi hai mắt `unknown` và chưa vẽ `face13`, vì không đặt được các điểm mắt. Theo 4.6 có lẽ vẫn phải vẽ và đánh cờ `outside`; cần guideline nói rõ.
3. Câu hỏi: Đồ vật ở ghế phụ trên ảnh DMS (túi, hộp) có nên gán để dùng cho OMS hàng ghế trước không? Hiện 4.2 cấm vẽ `object` trên DMS.
   Khung ví dụ: G10 D01, D03, D05 (túi ở ghế `front_passenger`).
   Bạn tạm gán thế nào và vì sao: Mình đã vẽ `object` vì quen tay từ phần OMS. Đây là lỗi theo 4.2; nếu muốn OMS ghế trước học được đồ vật thì cần một nguồn ảnh có camera nhìn ghế phụ.
4. Câu hỏi: Cùng một cảnh, ảnh RGB và ảnh IR thấy được thông tin khác nhau. Nhãn mỗi ảnh gán độc lập, hay được dùng ảnh rõ hơn để gán cho ảnh kia?
   Khung ví dụ: G10 S03_rgb / S03_ir, `seatbelt` người ghế giữa.
   Bạn tạm gán thế nào và vì sao: Theo 4.4-1 mỗi ảnh gán độc lập. Mình ghi `unknown` ở RGB nhưng lại ghi `not_worn` ở IR dù IR khó nhìn hơn, nên IR đúng ra phải là `unknown`.
