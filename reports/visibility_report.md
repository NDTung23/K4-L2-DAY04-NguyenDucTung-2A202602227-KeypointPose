# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 16.11 khớp có v > 0 mỗi người
- Tổng: v=2 324 | v=1 127 | v=0 25

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 6 | 0 | 21% |
| 1 | left_eye | 22 | 6 | 0 | 21% |
| 2 | right_eye | 21 | 7 | 0 | 25% |
| 3 | left_ear | 13 | 15 | 0 | 54% |
| 4 | right_ear | 17 | 11 | 0 | 39% |
| 5 | left_shoulder | 24 | 4 | 0 | 14% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 24 | 4 | 0 | 14% |
| 8 | right_elbow | 25 | 3 | 0 | 11% |
| 9 | left_wrist | 18 | 10 | 0 | 36% |
| 10 | right_wrist | 19 | 8 | 1 | 29% |
| 11 | left_hip | 18 | 10 | 0 | 36% |
| 12 | right_hip | 21 | 7 | 0 | 25% |
| 13 | left_knee | 15 | 10 | 3 | 36% |
| 14 | right_knee | 16 | 9 | 3 | 32% |
| 15 | left_ankle | 13 | 6 | 9 | 21% |
| 16 | right_ankle | 9 | 10 | 9 | 36% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
