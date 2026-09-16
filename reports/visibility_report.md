# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 30 skeleton, trung bình 16.0 khớp có v > 0 mỗi người
- Tổng: v=2 328 | v=1 152 | v=0 30

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 7 | 0 | 23% |
| 1 | left_eye | 21 | 9 | 0 | 30% |
| 2 | right_eye | 21 | 9 | 0 | 30% |
| 3 | left_ear | 13 | 17 | 0 | 57% |
| 4 | right_ear | 15 | 15 | 0 | 50% |
| 5 | left_shoulder | 25 | 5 | 0 | 17% |
| 6 | right_shoulder | 28 | 2 | 0 | 7% |
| 7 | left_elbow | 24 | 6 | 0 | 20% |
| 8 | right_elbow | 25 | 5 | 0 | 17% |
| 9 | left_wrist | 20 | 10 | 0 | 33% |
| 10 | right_wrist | 21 | 8 | 1 | 27% |
| 11 | left_hip | 18 | 11 | 1 | 37% |
| 12 | right_hip | 15 | 14 | 1 | 47% |
| 13 | left_knee | 17 | 8 | 5 | 27% |
| 14 | right_knee | 17 | 9 | 4 | 30% |
| 15 | left_ankle | 13 | 8 | 9 | 27% |
| 16 | right_ankle | 12 | 9 | 9 | 30% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
