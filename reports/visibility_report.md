# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.38 khớp có v > 0 mỗi người
- Tổng: v=2 341 | v=1 105 | v=0 47

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 6 | 1 | 21% |
| 1 | left_eye | 20 | 8 | 1 | 28% |
| 2 | right_eye | 20 | 8 | 1 | 28% |
| 3 | left_ear | 13 | 15 | 1 | 52% |
| 4 | right_ear | 14 | 14 | 1 | 48% |
| 5 | left_shoulder | 27 | 2 | 0 | 7% |
| 6 | right_shoulder | 26 | 2 | 1 | 7% |
| 7 | left_elbow | 25 | 3 | 1 | 10% |
| 8 | right_elbow | 24 | 4 | 1 | 14% |
| 9 | left_wrist | 21 | 5 | 3 | 17% |
| 10 | right_wrist | 20 | 7 | 2 | 24% |
| 11 | left_hip | 21 | 8 | 0 | 28% |
| 12 | right_hip | 25 | 4 | 0 | 14% |
| 13 | left_knee | 16 | 6 | 7 | 21% |
| 14 | right_knee | 15 | 6 | 8 | 21% |
| 15 | left_ankle | 15 | 5 | 9 | 17% |
| 16 | right_ankle | 17 | 2 | 10 | 7% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
