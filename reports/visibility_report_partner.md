# Visibility report

- Thư mục nhãn: `..\..\cvat-day2\train`
- 20 ảnh, 28 skeleton, trung bình 13.5 khớp có v > 0 mỗi người
- Tổng: v=2 330 | v=1 48 | v=0 98

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 0 | 4 | 0% |
| 1 | left_eye | 20 | 1 | 7 | 4% |
| 2 | right_eye | 22 | 0 | 6 | 0% |
| 3 | left_ear | 10 | 5 | 13 | 18% |
| 4 | right_ear | 15 | 3 | 10 | 11% |
| 5 | left_shoulder | 25 | 3 | 0 | 11% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 24 | 2 | 2 | 7% |
| 8 | right_elbow | 23 | 4 | 1 | 14% |
| 9 | left_wrist | 19 | 4 | 5 | 14% |
| 10 | right_wrist | 18 | 5 | 5 | 18% |
| 11 | left_hip | 19 | 7 | 2 | 25% |
| 12 | right_hip | 19 | 7 | 2 | 25% |
| 13 | left_knee | 17 | 1 | 10 | 4% |
| 14 | right_knee | 17 | 2 | 9 | 7% |
| 15 | left_ankle | 15 | 2 | 11 | 7% |
| 16 | right_ankle | 16 | 1 | 11 | 4% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
