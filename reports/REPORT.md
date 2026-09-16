# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Minh Tú   Nhóm: Cá nhân   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 315 / 154 / 24 |
| Thời gian trung bình mỗi ảnh |4-5 phút|

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` - 52% (15/29 skeleton bị che)
2. `right_ear` - 52% (15/29 skeleton bị che)
3. `left_eye` / `right_wrist` - 38% (đồng hạng ba, 10-11/29 skeleton bị che)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng. Tai là khớp khó nhất vì tóc và mũ bảo hiểm che gần nửa số người trong ảnh - đây
cũng là lý do mục 2 của `GUIDELINE_MINI.md` phải có luật riêng cho tai. Cổ tay đứng
thứ ba vì nhiều người ở tư thế tay đưa ra sau lưng hoặc sau vô lăng, còn mắt bị che
chủ yếu do góc nghiêng đầu hoặc kính râm. Cả ba đều khớp với cảnh báo mà
`tools/check_pose_labels.py` từng nêu ở Chặng 3, chứ không phải cảm giác chủ quan.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.900 | 0.928 |
| OKS@0.50 | 0.966 | 1.000 |
| OKS@0.75 | 0.966 | 1.000 |
| Lỗi `dao_trai_phai` | 1 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

> "Trước rework" là lần chạy đầu (script báo "1 skeleton cần sửa trước: train_10.jpg").
> "Sau rework" là lần chạy lại sau khi sửa `train_10.txt` - script xác nhận "Không có
> skeleton nào cần rework: mọi người đều đạt OKS >= 0.75 và không có lỗi đã phân loại."

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_10.jpg`, người #1: đảo lại toàn bộ cặp trái/phải của skeleton (đúng như gợi ý
  "Đổi lại toàn bộ cặp trái/phải cho skeleton này thì OKS tăng hẳn" trong
  `outputs/eval_vs_gold.json`), sau đó chỉnh lại các khớp đang "Trượt hẳn":
  `right_eye`, `right_ear`, `left_shoulder`, `right_shoulder`, `left_elbow`, `right_elbow`,
  `left_wrist`, `right_wrist` về đúng vị trí giải phẫu trên ảnh gốc/`outputs/vis_train`.
- Chạy lại `tools/evaluate_pose_annotations.py`: OKS của `train_10.jpg` người #1 tăng từ
  0.119 lên mức không còn bị gắn cờ, không còn skeleton nào "cần sửa trước".

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

`train_10.jpg`, người thứ 1 (gold_person 1) - OKS chỉ 0.1191, kèm hàng loạt khớp
`truot_han` (vai, khuỷu tay, cổ tay lệch 180-320px). Đây là ảnh **khó**, không phải ảnh
dễ: người có tư thế phức tạp và nhiều khớp bị che, đúng như cảnh báo đảo trái/phải mà
`tools/check_pose_labels.py` đã nêu ở Chặng 3 cho `train_10.txt:1`.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:


-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | 0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | 0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` **tăng** nhẹ +0.0055 sau fine-tune, không giảm. 20 ảnh của tôi không làm
   hỏng gì so với baseline COCO; mức tăng nhỏ vì chỉ có 20 ảnh train nên model gần như giữ
   nguyên khả năng đã học từ COCO, chỉ tinh chỉnh thêm chút ít trên các khớp hay bị che
   (tai, cổ tay) mà tập của tôi có nhiều ví dụ hơn.

2. `box_mAP50-95` (0.8041 sau fine-tune) cao hơn `pose_mAP50-95` (0.6908) khoảng **0.11
   điểm**. Model tìm *người* (box) dễ hơn tìm *khớp* (pose) đáng kể, vì phát hiện một
   bounding box chỉ cần định vị đúng vùng chứa người, còn dự đoán khớp đòi hỏi định vị
   chính xác 17 điểm nhỏ, nhiều điểm trong số đó bị che hoặc ở tư thế bất thường.

3. _(chưa trả lời được - mục 5 của notebook `day4_pose_finetune_yolo26.ipynb` chưa được
   chạy trong repo này: các cell dự đoán trên 10 ảnh test đều có `outputs: []`, nên chưa có
   ảnh model đoán sai cụ thể để phân loại theo bốn kiểu lỗi của slide 43)_

4. _(chưa trả lời được - mục 6 của notebook (so OKS nhãn-của-bạn vs model) cũng chưa chạy,
   nên chưa có bảng OKS theo ảnh để tìm ảnh thấp nhất)_

5. _(phụ thuộc câu 4 - cần chạy notebook trước, sau đó so với ảnh gán tệ nhất trong
   `outputs/eval_vs_gold.json`, hiện là `train_10.jpg` với OKS 0.1191)_

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

Ảnh `train_10`, người thứ 1, khớp `right_ankle`. Khi soi trong `outputs/vis_train/train_10.jpg`,
chân của người này bị cắt ngay tại mép dưới khung ảnh, không còn phần ống chân hay bàn chân
nào lộ ra ở phía trong ảnh để ước lượng vị trí. Vì phần cơ thể liền kề (đầu gối) cũng đã nằm
sát mép, tôi kết luận cổ chân thực sự nằm ngoài khung chứ không phải bị vật khác che, nên
gán `v=0` và không đặt chấm, thay vì `v=1` như checker từng cảnh báo có thể nhầm ở các ảnh
khác (`train_04`, `train_10` mục 2 của `GUIDELINE_MINI.md`).
