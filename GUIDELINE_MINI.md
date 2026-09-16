# Mini guideline - nhóm: Cá nhân  |  người gán: Nguyen Minh Tu  |  ngày: 16/09/2026

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
| Hông của người mặc quần áo dài | Vẫn đặt `left_hip` và `right_hip` tại vị trí ước lượng theo hai bên xương chậu; nếu quần áo che thì dùng `v = 1`, không dùng `v = 0`. Ảnh mẫu: [train_02](outputs/vis_train/train_02.jpg). | Hông thường không có bề mặt nhìn thấy, nhưng người vẫn còn trong khung. Giữ đủ 17 điểm giúp model học được vị trí giải phẫu thay vì hiểu nhầm là điểm nằm ngoài ảnh. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Đặt chấm vào vị trí tai ước lượng và gắn `v = 1`; chỉ dùng `v = 2` khi thấy rõ vị trí tai. Ảnh mẫu: [train_03](outputs/vis_train/train_03.jpg). | Tai có `%v=1` cao nhất trong report (52% cho cả hai bên), nên cần thống nhất rằng bị tóc/mũ che không đồng nghĩa với Outside. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp nằm ngoài mép ảnh gắn `v = 0` và không đặt chấm; các khớp còn trong ảnh vẫn phải có chấm và dùng `v = 1` nếu bị che. Ảnh mẫu: [train_10](outputs/vis_train/train_10.jpg). | Phân biệt đúng Outside với Occluded, tránh xoá nhầm các khớp vẫn có thể ước lượng trong vùng ảnh. |
| Cổ tay nằm sau tay lái / sau thân mình | Vẫn đặt cổ tay tại vị trí ước lượng, gắn `v = 1` nếu cổ tay còn trong khung; không đổi trái/phải theo phía xuất hiện trên ảnh. Ảnh mẫu: [train_04](outputs/vis_train/train_04.jpg). | Cổ tay là khớp thường bị che; report có `right_wrist` v=0 nên cần kiểm tra lại xem có thật sự ra ngoài ảnh hay chỉ bị che. |
| Hai người chồng lên nhau | Gán xong toàn bộ 17 điểm cho từng người riêng biệt. Điểm bị người kia che vẫn đặt theo giải phẫu của người đang gán và dùng `v = 1`; không kéo điểm sang cơ thể người bên cạnh. Ảnh mẫu: [train_16](outputs/vis_train/train_16.jpg). | Tránh lỗi nhầm người và giữ đúng skeleton của từng người khi hai cơ thể chồng lấn. |
| Người nhỏ đến mức nào thì không gán nữa | Không bỏ người trong 20 ảnh core vì bộ dữ liệu đã được chọn để gán đủ. Nếu người nhỏ hoặc bị khuất, vẫn tạo skeleton đủ 17 điểm và dùng cờ visibility phù hợp. Ảnh mẫu: [train_10](outputs/vis_train/train_10.jpg). | Luật lớp yêu cầu mọi người trong ảnh đều có đủ 17 điểm; không tự đặt ngưỡng bỏ người. |


## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_02`, người thứ `1`, khớp `left_shoulder/right_shoulder` và `left_hip/right_hip`

- Mơ hồ ở chỗ nào: Trong ảnh visualize, hai đường vai và hông cắt chéo; cảnh báo cho thấy trái/phải có thể đã bị gán theo phía ảnh thay vì theo cơ thể.
- Bạn quyết thế nào: Kiểm tra lại bằng hướng cơ thể của người đó, giữ `left_*` ở bên trái của chính người đó và `right_*` ở bên phải của chính người đó.
- Vì sao: Luật bắt buộc tính trái/phải theo cơ thể người, không theo vị trí trên ảnh. Nếu chưa chắc, ưu tiên kiểm tra mắt và hướng vai/hông cùng nhau.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học quan hệ trái/phải đảo ngược; augmentation lật ảnh sẽ làm lỗi này xuất hiện thêm lần nữa.

### Ca 2 - ảnh `train_04`, người thứ `2`, khớp `wrist/hip/knee/ankle` có `v = 0`

- Mơ hồ ở chỗ nào: Người nằm gọn giữa ảnh nhưng có 4 khớp bị gắn `v = 0`, trong khi không có dấu hiệu các khớp đó ra ngoài mép ảnh.
- Bạn quyết thế nào: Đặt lại các điểm ở vị trí giải phẫu ước lượng và đổi sang `v = 1` nếu chúng bị che; chỉ giữ `v = 0` cho khớp thật sự nằm ngoài khung.
- Vì sao: `v = 0` làm khớp bị loại khỏi OKS, còn luật lớp yêu cầu khớp bị che nhưng còn trong ảnh phải được giữ lại.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model nhận ít tín hiệu hơn ở các khớp bị che và đánh giá chất lượng pose thấp không cần thiết.

### Ca 3 - ảnh `train_10`, người thứ `1`, khớp `wrist/hip/knee/ankle` có `v = 0`

- Mơ hồ ở chỗ nào: Có 4 khớp `v = 0` dù toàn thân người vẫn nằm trong ảnh; khó phân biệt khớp bị che với khớp bị đặt sai vị trí.
- Bạn quyết thế nào: Soi lại ảnh gốc và ảnh visualize ở vùng khớp; nếu vị trí còn nằm trong ảnh thì đặt chấm ước lượng và gắn `v = 1`.
- Vì sao: Đây là đúng trường hợp cảnh báo của checker: người nằm gọn trong ảnh không nên có nhiều khớp Outside.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học rằng khớp có thể biến mất khi bị che, làm giảm khả năng dự đoán khớp khuất.

## 4. Sau khi so visibility report với bạn cùng nhóm

- **Khớp lệch `%v=1` nhiều nhất:** `left_ear` (bạn `59%` / họ `0%`).
- **Nguyên nhân là guideline chưa rõ hay một trong hai bên gán sai:** Bên đối chiếu chưa nộp
  nhãn (thư mục đối chiếu có 0 skeleton, `%v=1` bằng `0%`), hoặc bên đối chiếu đang xoá nhầm
  các điểm bị tóc che (`v = 0`) thay vì giữ chấm ước lượng (`v = 1`).
- **Luật mới bổ sung vào mục 2 sau khi thống nhất:** Với tai bị tóc/mũ che nhưng đầu vẫn còn
  trong khung, bắt buộc phải chấm phỏng đoán dựa theo trục mắt và gán `v = 1`, nghiêm cấm xoá
  điểm hoặc để `v = 0`.
