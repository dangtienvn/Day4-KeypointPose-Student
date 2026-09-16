# Mini guideline - nhóm: KeypointPose  |  người gán: Học viên  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Ước lượng vị trí ngang thắt lưng/khớp hông giải phẫu, gắn `v = 1` | Hông bị áo/quần che nhưng cơ thể vẫn nằm trong khung ảnh, model cần learn tỉ lệ thân |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Ước lượng vị trí tai dưới quai/nón bảo hiểm, gắn `v = 1` | Vùng đầu có mũ/tóc che nhưng vị trí tai vẫn nằm trong chu vi đầu |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp từ đùi/đầu gối/cổ chân lọt ra ngoài mép ảnh set `v = 0` | Khớp nằm ngoài khung ảnh không được đặt chấm để tránh gây sai lệch tọa độ |
| Cổ tay nằm sau tay lái / sau thân mình | Ước lượng vị trí bàn tay/cổ tay trên tay lái, gắn `v = 1` | Tay lái xe che khuất cổ tay nhưng tay vẫn nằm trong khung hình |
| Hai người chồng lên nhau | Gán đủ 17 điểm cho từng người độc lập, không nối dây chéo giữa 2 người | Tránh lỗi nhầm người (cross-person annotation) khiến model học sai skeleton |
| Người nhỏ đến mức nào thì không gán nữa | Gán tất cả người có thể nhận diện được cấu trúc thân trong bộ 20 ảnh core | Đảm bảo độ bao phủ skeleton theo đúng yêu cầu bài lab |

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_02.jpg`, người thứ `1` (cyclist), khớp `nose`, `left_hip`, `right_hip`

- Mơ hồ ở chỗ nào: Người đi xe đạp đội nón bảo hiểm che khuất tai/mặt nghiêng, nón trùm kín đầu; hông bị áo quần đua xe che.
- Bạn quyết thế nào: Kéo `nose` xuống đúng sống mũi bên dưới kính mát (không đặt lên vỏ mũ bảo hiểm). Đặt 2 điểm hông (`left_hip`, `right_hip`) ở vị trí ước lượng thắt lưng và gắn `v = 1`.
- Vì sao: Mũi nằm ở mặt phía trước; hông bị che bởi trang phục nhưng người nằm gọn trong khung ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đặt `nose` lên đỉnh mũ, model học sai vị trí khuôn mặt. Nếu xóa điểm hông (`v = 0`), model bị mất thông tin tỉ lệ thân người.

### Ca 2 - ảnh `train_14.jpg`, người thứ `1` (cậu bé áo hoodie), khớp `left_hip`, `right_hip`, `left_wrist`

- Mơ hồ ở chỗ nào: Cậu bé đứng cạnh chị gái đi xe máy, phần hông bị xe và áo hoodie che, tay trái gần người bên cạnh.
- Bạn quyết thế nào: Giữ điểm `left_wrist` nằm gọn trên cánh tay áo xám của cậu bé, không đâm dây đỏ sang người chị gái. Đặt 2 điểm hông ở viền thắt lưng và chọn `v = 1`.
- Vì sao: Tránh lỗi gán nhầm người (cross-person annotation). Hông nằm trong ảnh nhưng bị xe che nên chọn `v = 1`.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu nối dây chéo sang người bên cạnh, model sẽ học sai cấu trúc skeleton (nối xương từ người này sang người khác).

### Ca 3 - ảnh `train_16.jpg`, người thứ `2` (cầu thủ áo trắng #15 GER Ultimate), khớp `right_shoulder`, `right_wrist`

- Mơ hồ ở chỗ nào: Cầu thủ giơ tay bắt đĩa ném, tư thế quay lưng hoàn toàn về phía camera.
- Bạn quyết thế nào: Tính Trái/Phải theo **lưng của cơ thể người**. Tay giơ cao bên phải ảnh là `RIGHT_ARM` (vì quay lưng), tay bên trái ảnh là `LEFT_ARM`.
- Vì sao: Trái/phải luôn tính theo cơ thể người. Khi quay lưng thì bên phải ảnh chính là tay phải của người đó.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu gán theo góc nhìn bức ảnh (coi tay phải là tay trái), model sẽ học sai vĩnh viễn khi gặp tư thế người quay lưng.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `52%` / họ `40%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline nhóm thống nhất chọn `v = 1` cho các trường hợp tai bị nón bảo hiểm hoặc tóc che một phần nhưng vẫn ước lượng được chu vi đầu.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Khớp bị che bởi mũ bảo hiểm/quần áo nếu còn nằm trong khung hình thì luôn ưu tiên đặt chấm ước lượng và tick `v = 1`.
