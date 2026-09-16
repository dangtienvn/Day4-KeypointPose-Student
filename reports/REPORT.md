# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Đặng Thanh Tiến Nhóm: KeypointPose Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

| Chỉ số                       |        Giá trị |
| ---------------------------- | -------------: |
| Số ảnh đã gán                |             20 |
| Số skeleton                  |             29 |
| v=2 / v=1 / v=0              | 341 / 105 / 47 |
| Thời gian trung bình mỗi ảnh |       4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` (52%)
2. `right_ear` (48%)
3. `left_hip` (28%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Trả lời: Đúng một phần. `left_ear` và `right_ear` có tỉ lệ `v=1` cao do nhân vật thường đội nón bảo hiểm hoặc tóc che khuất tai. Tuy nhiên, khớp khó xác định vị trí giải phẫu nhất thực tế lại là `left_hip` và `right_hip` vì phần hông bị áo/quần dài che phủ hoàn toàn, bắt buộc người gán nhãn phải ước lượng dựa trên khung xương thắt lưng chứ không nhìn thấy bề mặt trực tiếp.

## 2. Chấm với gold

| Chỉ số                | Trước rework | Sau rework |
| --------------------- | -----------: | ---------: |
| OKS trung bình        |        0.870 |      0.870 |
| OKS@0.50              |        0.931 |      0.931 |
| OKS@0.75              |        0.931 |      0.931 |
| Lỗi `dao_trai_phai`   |            3 |          3 |
| Lỗi `nham_nguoi`      |            3 |          3 |
| Lỗi `xoa_khop_bi_che` |            3 |          3 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- `train_02.jpg` (người #1): Sửa lại điểm `nose` từ đỉnh mũ bảo hiểm về sống mũi phía trước; sửa cờ `left_hip`/`right_hip` sang `v=1` và ước lượng vị trí thắt lưng.
- `train_04.jpg` (người #1): Sửa điểm `left_hip` tràn mép dưới ảnh từ `v=2` sang `v=0`.
- `train_14.jpg` (người #1): Gỡ đường nối chéo `left_wrist` đâm sang cơ thể người bên cạnh, giữ xương nằm trọn trên người áo xám.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái/phải ghi nhận tại `train_03.jpg`, `train_13.jpg`, `train_19.jpg`. Các ảnh này có tư thế người quay lưng hoặc di chuyển nhanh. Trong đó `train_13` và `train_19` là tư thế quay lưng/nghiêng dễ bị lầm tưởng góc nhìn của bức ảnh thay vì góc nhìn cơ thể người.

## 3. Kiểm chéo

Bạn cùng nhóm: Partner_A

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp        | Bạn |  Họ | Lệch | Nguyên nhân (guideline hay gán sai?)        |
| ----------- | --: | --: | ---: | ------------------------------------------- |
| `left_ear`  | 52% | 40% |  12% | Guideline chưa thống nhất về tai bị nón che |
| `right_ear` | 48% | 38% |  10% | Guideline chưa thống nhất về tai bị nón che |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Tai hoặc hông bị che bởi trang phục/mũ bảo hiểm nếu vẫn nằm trong phạm vi bức ảnh thì luôn ưu tiên đặt chấm ước lượng và gắn cờ `v=1` (Occluded).

## 4. Model

| Chỉ số         | yolo26n-pose gốc | Sau fine-tune |  Chênh |
| -------------- | ---------------: | ------------: | -----: |
| pose_mAP50     |            0.825 |         0.840 | +0.015 |
| pose_mAP50-95  |            0.612 |         0.625 | +0.013 |
| pose_precision |            0.850 |         0.862 | +0.012 |
| pose_recall    |            0.790 |         0.805 | +0.015 |
| box_mAP50-95   |            0.745 |         0.755 | +0.010 |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` tăng nhẹ khoảng +0.013 sau fine-tune. Việc fine-tune trên 20 ảnh giúp mô hình thích nghi tốt hơn với các tư thế che khuất đặc thù của bộ dữ liệu train.
2. `box_mAP` cao hơn `pose_mAP` khoảng 0.13. Điều này phản ánh mô hình định vị Bounding Box của người dễ hơn nhiều so với việc xác định chính xác 17 điểm khớp keypoint giải phẫu.
3. Trong ảnh test `test_02.jpg`, mô hình dự đoán trượt điểm cổ chân `left_ankle` $\rightarrow$ Lỗi được xếp vào loại **Trượt hẳn**.
4. Ảnh `train_03.jpg` có OKS thấp nhất giữa nhãn tự gán và mô hình dự đoán do góc nghiêng phức tạp và nón bảo hiểm che khuất. Nhãn tự gán chuẩn hơn nhờ ước lượng mắt người.
5. Ảnh gán khó nhất là `train_14.jpg` (hai người đứng đè lên nhau). Mô hình dự đoán cũng gặp khó khăn ở ảnh này, chứng tỏ nhiễu bối cảnh và việc che khuất chéo ảnh hưởng đồng thời đến cả người gán nhãn lẫn mô hình.

## 5. Một rule evidence bạn đã dùng

Tại ảnh `train_02.jpg` (người số 1 - người đi xe đạp), hai điểm `left_hip` và `right_hip` bị quần áo đua xe và yên xe che khuất hoàn toàn. Dựa vào viền thắt lưng và vị trí đùi đang đạp xe, cả hai khớp vẫn nằm trọn vẹn bên trong khung ảnh (chưa bị cắt lọt ra ngoài mép ảnh). Do đó, áp dụng đúng rule bài lab, tôi chọn trạng thái `v=1` (Occluded) và đặt 2 chấm ước lượng tại vị trí giải phẫu thắt lưng thay vì xóa điểm hoặc để `v=0`.
