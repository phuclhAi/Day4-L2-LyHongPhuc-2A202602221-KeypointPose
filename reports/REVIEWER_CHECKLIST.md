# Reviewer checklist - điền khi kiểm bài người khác

Người gán: `Nguyễn Thường Huy (bạn cùng nhóm)`   Người kiểm: `Lý Hồng Phúc`   Ngày: `16-09-2026`

Đã nhận nhãn gốc tại `cvat-day2/train/*.txt` (20 file) và chạy đủ 3 lệnh:

```bash
python tools/check_pose_labels.py --images dataset/images/train --labels ../../cvat-day2/train
python tools/visualize_pose.py --images dataset/images/train --labels ../../cvat-day2/train --out outputs/vis_review
python tools/visibility_report.py --labels ../../cvat-day2/train --out outputs/visibility_report_partner.json --markdown reports/visibility_report_partner.md
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | `check_pose_labels.py` báo "ĐẠT định dạng", 20/20 file đọc được, 28 skeleton |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | Không có cảnh báo đảo trái/phải nào (khác với bài của mình, vốn có ở `train_02`/`train_16`) |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑* | *Soi bằng mắt 4/20 ảnh (`train_01`, `train_02`, `train_04`, `train_11`) ở `outputs/vis_review/` - không thấy nhầm người. Chưa soi hết 20 ảnh |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☐ **KHÔNG ĐẠT** | Xác nhận bằng mắt: `train_02` cả 5 khớp mặt bị `v=0` dù đầu vẫn trong khung (chỉ quay đi); `train_04` người 2 cả hai tai `v=0` dù chỉ bị mũ bảo hiểm che - xem bảng lỗi bên dưới |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☐ **KHÔNG ĐẠT** | Có ca đúng thật (`train_01`: chân dưới hông ra ngoài mép ảnh - hợp lệ), nhưng cũng có ca sai rõ (`train_04` người 2: hông `v=0` dù thân trên vẫn hiển thị đầy đủ trong khung, chỉ bị ba lô/ghi đông che) |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑* | *Trong 4 ảnh đã soi, các điểm `v=2` đều nằm đúng vị trí khớp thật. Chưa soi hết 20 ảnh |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | Tự kiểm trực tiếp trên máy bạn cùng nhóm (không phải chạy tool ở đây, vì chỉ nhận bản YOLO `.txt`) |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | Kiểm bằng `awk '{print NF}'` trên cả 20 file - toàn bộ đều đúng 56 số/dòng |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☑ | `reports/visibility_compare.md` (so sánh) + `reports/visibility_report_partner.md` (báo cáo riêng của họ, tự chạy lại từ nhãn gốc - khớp đúng số liệu họ gửi ban đầu) |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | Tự kiểm trực tiếp trên máy bạn cùng nhóm - đã đọc `GUIDELINE_MINI.md` của họ |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | 0 lỗi chặn (`ĐẠT định dạng`), có 15 cảnh báo (không chặn) - xem bảng lỗi bên dưới |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_02` | 1 | `nose, left_eye, right_eye, left_ear, right_ear` | Cả 5 khớp mặt bị gắn `v=0` (không đặt chấm) trong khi đầu vẫn nằm trong khung - chỉ là quay đi, không phải ra khỏi ảnh | Đổi thành `v=1`, đặt chấm ước lượng theo hướng đầu đang quay |
| `train_04` | 2 (cô gái bên phải) | `left_hip, right_hip` | Hông bị gắn `v=0` dù thân trên vẫn hiển thị đầy đủ đến ngang hông - chỉ bị ba lô/ghi đông che | Đổi thành `v=1`, đặt chấm ước lượng giải phẫu |
| `train_04` | 1 và 2 | `left_ear, right_ear` | Cả 4 tai (2 người) bị gắn `v=0` vì mũ bảo hiểm che kín - che bởi vật thể khác `v=0` (ra khỏi khung) | Đổi thành `v=1` cho cả 4 |
| `train_11` | 1 | `left_wrist, right_wrist` | Cổ tay bị gắn `v=0`, nhưng người vẫn ngồi trong khung, tay chỉ bị con mèo trước mặt che khuất | Đổi thành `v=1`, ước lượng theo hướng cánh tay |

*(Tham khảo thêm, không tính lỗi: `train_01` cũng có 4 khớp chân `v=0` nhưng đã soi ảnh - ảnh
thật sự bị cắt ở mép dưới ngay dưới hông, nên `v=0` ở đây là **đúng**, không phải lỗi.)*

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: **lạm dụng `v=0` (Outside) cho khớp bị che, thay vì
  `v=1` (Occluded)** - đúng lỗi số 3 mà GUIDE/README mô tả. Xác nhận qua 15/20 file bị
  `check_pose_labels.py` cảnh báo, và soi bằng mắt 4 ảnh ở trên đều là ca "vẫn trong khung nhưng
  bị che" chứ không phải ca "thật sự ra khỏi ảnh". Riêng vùng mặt (mắt/mũi/tai khi quay đầu) và
  tai (khi đội mũ bảo hiểm) là hai vị trí lặp lại nhiều nhất.
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**? Nghiêng về lỗi **guideline chưa rõ**: các
  ca sai đều nhất quán theo một kiểu suy nghĩ ("không thấy được = Outside"), không phải bấm nhầm
  ngẫu nhiên - tức là người gán chưa phân biệt rõ "bị che" (`v=1`) và "ra khỏi khung hình" (`v=0`)
  là hai khái niệm khác nhau, chứ không theo vị trí pixel như thao tác đúng đã làm ở `train_01`.
