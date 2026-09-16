# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.17 khớp có v > 0 mỗi người
- Tổng: v=2 315 | v=1 154 | v=0 24

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 21 | 8 | 0 | 28% |
| 1 | left_eye | 18 | 11 | 0 | 38% |
| 2 | right_eye | 19 | 10 | 0 | 34% |
| 3 | left_ear | 14 | 15 | 0 | 52% |
| 4 | right_ear | 14 | 15 | 0 | 52% |
| 5 | left_shoulder | 23 | 6 | 0 | 21% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 21 | 8 | 0 | 28% |
| 8 | right_elbow | 25 | 4 | 0 | 14% |
| 9 | left_wrist | 19 | 10 | 0 | 34% |
| 10 | right_wrist | 17 | 11 | 1 | 38% |
| 11 | left_hip | 20 | 9 | 0 | 31% |
| 12 | right_hip | 19 | 9 | 1 | 31% |
| 13 | left_knee | 16 | 10 | 3 | 34% |
| 14 | right_knee | 16 | 10 | 3 | 34% |
| 15 | left_ankle | 13 | 8 | 8 | 28% |
| 16 | right_ankle | 13 | 8 | 8 | 28% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
