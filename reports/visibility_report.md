# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 344 | v=1 102 | v=0 30

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 6 | 0 | 21% |
| 1 | left_eye | 20 | 8 | 0 | 29% |
| 2 | right_eye | 20 | 8 | 0 | 29% |
| 3 | left_ear | 12 | 16 | 0 | 57% |
| 4 | right_ear | 15 | 12 | 1 | 43% |
| 5 | left_shoulder | 27 | 1 | 0 | 4% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 25 | 3 | 0 | 11% |
| 8 | right_elbow | 25 | 3 | 0 | 11% |
| 9 | left_wrist | 21 | 7 | 0 | 25% |
| 10 | right_wrist | 21 | 6 | 1 | 21% |
| 11 | left_hip | 19 | 9 | 0 | 32% |
| 12 | right_hip | 23 | 5 | 0 | 18% |
| 13 | left_knee | 16 | 6 | 6 | 21% |
| 14 | right_knee | 18 | 4 | 6 | 14% |
| 15 | left_ankle | 16 | 4 | 8 | 14% |
| 16 | right_ankle | 17 | 3 | 8 | 11% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
