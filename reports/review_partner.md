# Reviewer checklist - Kiểm chéo bài người khác

Người gán: Partner_A   Người kiểm: Học viên   Ngày: 16/09/2026

## Kết quả kiểm tra

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | Đã gán đủ 17 điểm |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | Đạt chuẩn |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | Không bị cross-person |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☑ | Đã gán chấm ước lượng |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☑ | Đạt chuẩn |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | Không dùng Hidden |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | Đủ 51 số |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | Đủ 56 số |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | Đã đặt cạnh nhau |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | Có 3 ca mơ hồ |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | 0 lỗi |

## Lỗi tìm được

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_02.jpg` | 1 | `nose` | Đặt nhầm lên mũ bảo hiểm | Kéo xuống sống mũi phía trước |
| `train_04.jpg` | 1 | `left_hip` | Tràn viền y > 1.0 để v=2 | Chuyển cờ sang v=0 (Outside) |
| `train_14.jpg` | 1 | `left_wrist` | Nối nhầm sang người bên cạnh | Thu về cánh tay áo xám |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: Lỗi nhầm lẫn cờ `v=0` và `v=1` ở các khớp bị trang phục/vật che khuất.
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**: Chủ yếu là lỗi guideline ban đầu chưa làm rõ quy tắc cờ visibility đối với các khớp bị che trong khung hình.
