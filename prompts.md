# Khai báo dùng AI — Vũ Tuyết Trinh (2A202603164)

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào. Trong các lần dưới đây, AI **không** xem ảnh và **không** gán nhãn.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Claude Code (Claude Opus 5.5) | Đọc tài liệu lab, kiểm tra môi trường; sửa CVAT không mở được ở localhost (đổi `CVAT_HOST` trong `.env`, chạy lại `docker compose`) | Dùng để mở được CVAT ở `localhost:8080` | Có. Mở `localhost:8080` trên trình duyệt |
| 2 | Claude Code | Chép thư mục G10 vào repo, gom 22 ảnh cốt lõi vào một thư mục để tạo task (AI chỉ xem tên file, không mở ảnh) | Tạo task `Day12-G10` với 22 ảnh | Có. Đếm số ảnh trong task trên CVAT |
| 3 | Claude Code | Hướng dẫn thao tác CVAT và tóm tắt Guideline Cabin v1.0 (thứ tự gán, cách vẽ box, phím tắt, Attribute annotation mode) | Tham khảo khi tự gán nhãn | Có. Đối chiếu với GUIDE.md §4 |
| 4 | Claude Code | Hỏi cách xử lý khi không kịp hạn nộp | Báo coach, ưu tiên lại thứ tự làm | — |
| 5 | Claude Code | Tạo thư mục nộp từ mẫu trong `templates/` | Điền nội dung của mình vào các mẫu | Có. Đối chiếu với SUBMISSION.md |
| 6 | Claude Code | Chạy `cabin_check` (cpA, cpB, final) trên export của mình, đọc XML để chỉ ra chỗ sai guideline; viết dòng giải thích WARN/lỗi đã biết ở cuối `check_final.txt`. AI không sửa nhãn | Biết lỗi còn lại; quyết định nộp bản hiện tại, ghi rõ lỗi đã biết | Có. Đối chiếu GUIDE §4.2, 4.6, 4.8 |
| 7 | Claude Code | AI hỏi mình câu hỏi trắc nghiệm về những gì mình quan sát khi gán, các lớp mình chọn và các ca khó; AI viết lại câu trả lời của mình thành `ir_log.csv`, `self_review.md`, `class_list_v0.md` | Nộp các file này sau khi tự đọc lại và sửa | Có. Đọc lại, đối chiếu với nhãn của mình trên CVAT |
| 8 | Claude Code | AI hiển thị nguyên văn các câu exit ticket dạng chọn đáp án để mình tự chọn, rồi ghi đáp án của mình vào `exit_ticket.md`; AI không gợi ý, không chấm đáp án. Ghi mục "Cá nhân" trong `TEAM.md` từ lựa chọn của mình | Nộp đúng đáp án mình chọn | Có. Đọc lại file |
