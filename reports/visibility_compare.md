# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 15.82 khớp có v > 0 mỗi người
- Tổng: v=2 340 | v=1 103 | v=0 33

So sánh với `cvat-day2/visibility_report (1).md (bao cao da tong hop, khong phai thu muc nhan goc)` (28 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 3 | left_ear | 50% | 18% | 32 |
| 4 | right_ear | 36% | 11% | 25 |
| 9 | left_wrist | 36% | 14% | 21 |
| 0 | nose | 21% | 0% | 21 |
| 1 | left_eye | 25% | 4% | 21 |
| 2 | right_eye | 21% | 0% | 21 |
| 16 | right_ankle | 21% | 4% | 18 |
| 14 | right_knee | 21% | 7% | 14 |
| 10 | right_wrist | 29% | 18% | 11 |
| 13 | left_knee | 14% | 4% | 11 |
| 7 | left_elbow | 14% | 7% | 7 |
| 11 | left_hip | 18% | 25% | 7 |
| 8 | right_elbow | 11% | 14% | 4 |
| 12 | right_hip | 29% | 25% | 4 |
| 5 | left_shoulder | 7% | 11% | 4 |
| 15 | left_ankle | 11% | 7% | 4 |
| 6 | right_shoulder | 4% | 4% | 0 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
